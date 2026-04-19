# How to Use Scarlot Skills

Two skill files live in the repo root:

- `scarlot-interview.skill` - analyzes interview transcripts, produces structured records, prepares sessions, runs synthesis
- `scarlot-sync.skill` - processes internal conversations (syncs, strategy sessions, planning calls) into structured records

---

## Installing in Claude Desktop

1. Open Claude Desktop
2. Click the Skills icon in the left sidebar (puzzle piece)
3. Click the **+** button at the top
4. Select the `.skill` file from the repo root
5. The skill appears in your Personal skills list with its description and trigger settings

Once added, the skill triggers automatically when your conversation matches its description, or you can invoke it via slash command.

## Installing in Claude Code (CLI)

Unzip each skill into `~/.claude/skills/`:

```bash
mkdir -p ~/.claude/skills/scarlot-interview
unzip scarlot-interview.skill -d ~/.claude/skills/scarlot-interview

mkdir -p ~/.claude/skills/scarlot-sync
unzip scarlot-sync.skill -d ~/.claude/skills/scarlot-sync
```

Claude Code picks them up automatically. No restart needed.

---

## What Each Skill Does

### scarlot-interview

Four modes:

| What you say | What happens |
|-------------|-------------|
| "Analyze this transcript" | Produces a 13-section structured record with evidence tiers, assumption tracking, and next actions |
| "Prepare for an interview with [context]" | Generates tailored questions based on current gaps. Reminds you of the Mom Test rules |
| "Run cross-interview synthesis" | Compares all records. Pattern matrix, assumption board, persona map, discovery readiness |
| "Update the assumption log" | Checks a new record against the 10 open assumptions |

When analyzing a transcript, paste it with this header:

```
Interviewee pseudonym: [NAME]
Date: [DATE]
Duration: [DURATION]
Language: [FR / EN / DE]

[PASTE TRANSCRIPT HERE]
```

Output goes to `docs/interviews/SCARLOT_INT_[YYYYMMDD]_[PSEUDONYM].md`

### scarlot-sync

Two modes:

| Mode | When to use |
|------|-------------|
| Full Record (8 sections) | Strategy sessions, major decisions, pivots |
| Summary (6 sections) | Routine syncs, standups, check-ins |

Defaults to Summary unless the conversation contains strategic decisions or priority changes.

Output goes to `docs/syncs/SCARLOT_SYNC_[YYYYMMDD]_[TOPIC].md`

---

## Tips

- After every interview record, the skill proposes updates to `docs/priority-evolution.md`
- Both skills follow the privacy rules: no real names, pseudonyms only
- The interview skill references `references/context.md` inside the skill folder for the priority stack and assumptions. Update that file if the product direction changes significantly.
