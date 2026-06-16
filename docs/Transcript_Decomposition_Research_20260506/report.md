# Transcript Decomposition: Techniques and Pipeline Design

**Research date:** 2026-05-06
**Scope:** Methods, libraries, and pipeline architectures for decomposing voice transcripts (one or many speakers) into discrete topics over time, with measures of duration, prevalence, and repetition. Output target: a Claude Code skill.

---

## Executive Summary

For typical interview-length and meeting-length transcripts (5K to 50K tokens), an LLM-driven pipeline running entirely inside Claude is the right default. It produces accurate topic segmentation, recurrence tracking, speaker attribution, and time-on-topic measures without any heavy ML dependency. Specialized libraries (BERTopic, pyannote, WhisperX) only become necessary when (a) the pipeline must process audio rather than text, (b) the corpus spans many transcripts and needs cross-document topic modeling, or (c) inputs exceed comfortable context windows.

Recommended stack for the skill:

| Layer | Default (LLM-only) | Optional escalation |
|-------|--------------------|---------------------|
| Input parsing | Regex parsing of speaker labels and timestamps | None |
| Segmentation | Claude with structured prompt | BERT-embedding TextTiling, or LoRA-tuned LLM |
| Topic labeling and recurrence | Claude in a single pass | BERTopic with c-TF-IDF over time |
| Temporal analysis | Computed from timestamps or turn counts | BERTopic dynamic topic modeling |
| Multi-speaker attribution | Claude | Topic-aware multi-turn dialogue models |
| Output | Structured JSON, markdown report, optional HTML viz | Plotly, Sankey, timeline heatmap |

The skill should default to the LLM-only path and surface the escalation path as documented references rather than runtime dependencies.

---

## Findings

### 1. Topic segmentation: classical methods are deprecated for this use case

TextTiling (Hearst 1997) and C99 are the historical baselines for segmenting documents by topic. Both rely on bag-of-words cohesion measures and break down on conversational data: TextTiling fails when one speaker monopolizes airtime, and both algorithms suffer from word-sparsity that requires ad-hoc smoothing. [1][2]

Sentence-BERT embeddings replace bag-of-words cohesion with contextual similarity and produce a 15.5% reduction in segmentation error on meeting datasets compared to TextTiling. [3] LLM-based segmentation, especially with LoRA fine-tuning and pause-duration cues, is the 2025 state of the art and now generates hierarchical tables of contents (multi-level topic and subtopic boundaries). [4][5]

For a single transcript handed to Claude, none of the embedding or fine-tuning machinery is needed. Claude can produce equivalent segmentation directly from a structured prompt that asks for topic boundaries, topic labels, and short rationales. The classical algorithms are only relevant if the user wants a deterministic, offline, no-LLM fallback.

### 2. Topic modeling: BERTopic wins for cross-document, but is overkill for single transcripts

For short conversational text, BERTopic and Top2Vec consistently outperform LDA and NMF, with one comparison reporting BERTopic at least 34.2% better than alternatives in clustering quality. [6][7] BERTopic's strength comes from contextual embeddings plus c-TF-IDF, and it scales to thousands of documents while supporting dynamic topic modeling: keywords are recalculated at each timestamp via c-TF-IDF, allowing topic evolution to be visualized with Plotly. [8][9]

This matters for a corpus of transcripts. For a single transcript, an LLM produces equally usable topic labels in one prompt without an embedding model, clustering pass, or hyperparameter tuning. The skill should treat BERTopic as a documented escalation for cross-transcript analysis, not as a runtime dependency.

### 3. Speaker diarization is upstream of this skill

Diarization answers "who spoke when" from raw audio. The 2025 default is WhisperX, which orchestrates Whisper transcription, Wav2Vec2 phoneme alignment for word-level timestamps, and pyannote.audio 3.x for diarization. [10][11][12] NeMo's Sortformer is an alternative. All three require GPU-friendly infrastructure and 16 kHz PCM input, and none handle overlapping speech well. [11]

The skill takes a transcript as input, so diarization sits upstream. The skill must handle three input shapes: speaker-labelled with timestamps, speaker-labelled without timestamps, and unlabelled monologue. The first is what WhisperX produces, so the skill should accept its output format natively (SRT, VTT, JSON with speaker fields).

### 4. Temporal measures: the four metrics that matter

The literature on dynamic topic modeling and meeting analysis converges on four useful time-aware measures: [8][13][14]

- **Duration on topic** — wall-clock time or turn count spent on each topic block.
- **Prevalence** — share of total transcript time or turns occupied by each topic.
- **Recurrence** — number of distinct returns to a topic after it was left.
- **Burstiness** — whether a topic was discussed continuously or in scattered bursts.

These can all be computed deterministically once segmentation has assigned each turn or timestamped span to a topic ID. No specialized library is needed for the math; only segmentation quality matters.

### 5. Multi-speaker dynamics: ownership signals are useful and underused

Recent research treats topic introduction and topic transition as first-class signals. [15][16][17] Useful per-topic attributions:

- **Introducer** — who first surfaced the topic.
- **Returner** — who brought it back after the conversation moved on.
- **Time share** — speaking time per speaker within the topic.
- **Initiator-responder ratio** — does one speaker steer while others react.

Claude can produce all four directly from a structured prompt; the difficulty is not modeling but specification. The skill should require the LLM to attribute each topic block to a speaker and flag whether that block opened, continued, or returned to a topic.

### 6. Visualization: Sankey for flow, heatmap for prevalence, ribbon for evolution

Sankey diagrams are the standard for showing topic flow and transitions over time, with arrow widths proportional to time-on-topic. [18][19] BERTopic's Plotly visualizations cover dynamic topics over time as ribbons. [9] Timeline heatmaps work well for prevalence by speaker.

The skill should produce a markdown report by default and offer optional HTML output using a single self-contained file with embedded D3 or Plotly. No backend required.

### 7. Open-source full pipelines exist but optimize for transcription, not decomposition

Meetily, AI-Powered Meeting Summarizer, and Transcript Seeker are end-to-end stacks for "audio in, summary out". [20][21][22] None of them produce structured topic decomposition with recurrence and prevalence metrics in the form the user wants. They are useful as audio-to-transcript front ends; the skill picks up where they leave off.

### 8. Pipeline architecture for the skill

```
┌─────────────────────────┐
│ Input: transcript text  │  Speaker-labelled, timestamped, neither, or both
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ Parse                   │  Detect speaker labels + timestamps via regex
│                         │  Normalize to canonical structure
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ Segment + Label         │  Single LLM call: produce topic blocks with
│                         │  start/end indices, label, summary, speaker mix
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ Cluster recurrences     │  Group topic blocks that refer to the same
│                         │  subject; assign canonical topic IDs
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ Compute metrics         │  Duration, prevalence, recurrence count,
│                         │  burstiness, speaker attribution
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ Render                  │  Structured JSON + markdown report
│                         │  Optional self-contained HTML with Sankey
└─────────────────────────┘
```

---

## Recommendations for the Skill

1. **Default path is LLM-only.** No Python, no model downloads, no embeddings. Claude does parsing, segmentation, labeling, recurrence clustering, and metric computation in one or two prompts.
2. **Accept three input shapes** and detect which one applies: speaker-labelled with timestamps, speaker-labelled without timestamps, plain text.
3. **Output a canonical JSON schema** with topics, blocks, speakers, and metrics, plus a markdown report. JSON is the durable artifact; markdown is for humans.
4. **Keep recurrence as a first-class concept**, separate from segmentation. A topic block is a contiguous span; a topic is the canonical subject across all its blocks. Distinguish them in the schema.
5. **Compute four metrics per topic**: duration, prevalence, recurrence count, burstiness. Per-speaker breakdowns when speakers are labelled.
6. **Document escalation paths** for users who need (a) audio input, (b) cross-transcript topic modeling, or (c) very long transcripts. Point to WhisperX, BERTopic, and chunked-summarize-then-merge respectively. Do not bundle them.
7. **Optional HTML output** as a single self-contained file using vanilla CSS plus a minimal embedded chart. Do not add a build step.
8. **Be language-agnostic.** Scarlot transcripts are French, English, German. The LLM-only path handles this transparently; classical methods do not.

---

## Limitations and Caveats

- LLM-only segmentation cost scales with transcript length. Above roughly 100K tokens of transcript, a chunked map-reduce approach is needed: segment chunks separately, then reconcile topic IDs across chunks.
- Recurrence detection by an LLM is fuzzier than embedding-based clustering. For research-grade reproducibility, the BERTopic escalation path is more defensible.
- Without timestamps, "duration" reduces to turn count or character count. The skill should be explicit about which unit is in use.
- Diarization quality dominates downstream attribution. If the input transcript has wrong speaker labels, no topic-decomposition method recovers the truth.

---

## Bibliography

[1] Hearst, M. *TextTiling: Segmenting Text into Multi-paragraph Subtopic Passages*. Computational Linguistics, 1997. https://aclanthology.org/J97-1003.pdf
[2] Banerjee, S. and Rudnicky, A. *A TextTiling Based Approach to Topic Boundary Detection in Meetings*. Interspeech 2006. https://www.isca-archive.org/interspeech_2006/banerjee06_interspeech.pdf
[3] Solbiati, A. et al. *Unsupervised Topic Segmentation of Meetings with BERT Embeddings*. arXiv:2106.12978, 2021. https://arxiv.org/abs/2106.12978
[4] Freisinger et al. *Towards Multi-Level Transcript Segmentation*. Interspeech 2025. https://www.isca-archive.org/interspeech_2025/freisinger25_interspeech.pdf
[5] Valaboju, C. *Topic Segmentation using Large Language Models*. Master's thesis, 2025. https://downloads.webis.de/theses/papers/chaitanya-valaboju_2025.pdf
[6] Egger, R. and Yu, J. *A Topic Modeling Comparison Between LDA, NMF, Top2Vec, and BERTopic to Demystify Twitter Posts*. Frontiers in Sociology, 2022. https://www.frontiersin.org/journals/sociology/articles/10.3389/fsoc.2022.886498/full
[7] *Experimental Comparison of Three Topic Modeling Methods with LDA, Top2Vec and BERTopic*. Springer, 2024. https://link.springer.com/chapter/10.1007/978-981-99-9109-9_37
[8] Grootendorst, M. *Dynamic Topic Modeling with BERTopic*. Documentation. https://maartengr.github.io/BERTopic/getting_started/topicsovertime/topicsovertime.html
[9] Grootendorst, M. *Visualize Topics over Time*. BERTopic docs. https://maartengr.github.io/BERTopic/getting_started/visualization/visualize_topics.html
[10] Bain, M. *WhisperX: Automatic Speech Recognition with Word-level Timestamps and Diarization*. GitHub. https://github.com/m-bain/whisperX
[11] *Whisper and Pyannote: The Ultimate Solution for Speech Transcription*. Scalastic, 2025. https://scalastic.io/en/whisper-pyannote-ultimate-speech-transcription/
[12] *pyannote/speaker-diarization-3.1*. Hugging Face. https://huggingface.co/pyannote/speaker-diarization-3.1
[13] *Rethinking Meeting Effectiveness: A Benchmark and Framework for Temporal Fine-grained Automatic Meeting Effectiveness Evaluation*. arXiv 2026. https://arxiv.org/html/2604.17260
[14] *Topic Modeling Techniques for 2026: Seeded Modeling, LLM Integration, and Data Summaries*. Towards Data Science, 2026. https://towardsdatascience.com/topic-modeling-techniques-for-2026-seeded-modeling-llm-integration-and-data-summaries/
[15] *Topic-Aware Multi-turn Dialogue Modeling*. AAAI 2021. https://cdn.aaai.org/ojs/17668/17668-13-21162-1-2-20210518.pdf
[16] *An Empirical Study of Topic Transition in Dialogue*. CODI 2022. https://aclanthology.org/2022.codi-1.12.pdf
[17] *Topic Break Detection in Interview Dialogues Using Sentence Embedding of Utterance and Speech Intention*. PMC, 2022. https://pmc.ncbi.nlm.nih.gov/articles/PMC8780003/
[18] *Sankey diagram*. Wikipedia. https://en.wikipedia.org/wiki/Sankey_diagram
[19] García, D. *Unlock AI Conversation Flows with Sankey Diagrams*. Medium, 2025. https://iamdgarcia.medium.com/unlock-ai-conversation-flows-with-sankey-diagrams-boost-user-experience-1870c9a52c4f
[20] Zackriya Solutions. *Meetily: privacy-first local AI meeting assistant*. GitHub. https://github.com/Zackriya-Solutions/meeting-minutes
[21] Balayre, A. *AI-Powered Meeting Summarizer*. GitHub. https://github.com/AlexisBalayre/AI-Powered-Meeting-Summarizer
[22] Meeting BaaS. *transcript-seeker*. GitHub. https://github.com/meeting-baas/transcript-seeker
