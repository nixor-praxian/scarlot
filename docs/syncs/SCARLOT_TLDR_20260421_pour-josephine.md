---
date: 2026-04-21
pour: Joséphine
sujet: retours d'Andrew sur Maia / Scarlot
source: SCARLOT_SYNC_20260421_poc-focus-feasibility.md
---

# TL;DR pour Joséphine - Sync Philippe x Andrew (21 avril 2026)

Synthèse focalisée sur Maia / Scarlot et les commentaires d'Andrew, en préparation de notre prochaine session.

## 1. La discipline POC, validée

Andrew pousse fort dans le même sens que toi : plus rien en amont (étude de marché complémentaire, dimensionnement, deck) tant que le POC n'a pas tourné. Seule la traction du POC peut valider la thèse. Je m'engage : zéro dépense au-delà du strict nécessaire pour déployer et faire tourner le POC. Pas d'embauche, pas d'infra de luxe, pas de coût coulé.

## 2. Convergence produit (signal fort)

Après lecture des documents, Andrew arrive **indépendamment** à la même conclusion que celle écrite dans le brief Scarlot : pas de CRM web, pas de nouvelle plateforme, on vit dans WhatsApp où les contacts existent déjà, et on arrive naturellement vers un bot conversationnel. Andrew était le point potentiel de friction sur la direction produit ; il est désormais une voix de renforcement.

## 3. La grosse préoccupation d'Andrew : faisabilité économique

C'est le sujet sur lequel il n'est **pas** convaincu, et le plus important pour notre session.

- Un assistant type Claude est inutilisable en l'état pour une utilisatrice non-technique. Toute la couche en dessous (déploiement par utilisatrice, contrôle des coûts, garde-fous, isolation) est de l'infrastructure à construire. Scarlot vit *sous* la couche assistant.
- Topologie naïve : un VPS par utilisatrice = environ 6 CHF / utilisatrice / mois. À elle seule cette ligne dépasse probablement le revenu par utilisatrice dans ce marché.
- Topologie correcte : du **bare-metal partagé** (un host hébergeant plusieurs utilisatrices avec isolation forte). Vrai chantier d'ingénierie, mais pas insurmontable.
- Conséquence : les services cloud managés sont essentiellement exclus par les contraintes d'isolation.
- L'infra du POC ne sera **pas** l'infra de production. La transition est un work item à part entière qu'il faut planifier.

C'est le risque produit numéro un identifié par Andrew. Tout le reste découle de ça.

## 4. Concept produit qui ressort : Greylist + Blacklist

Sur la base de l'interview JUSTYNA (INT10) que je lui ai relayée (700 numéros bloqués, 50 à 100 messages reçus par jour, 5 à 10 RDV par semaine), Andrew formule un concept à deux étages :

- **Blacklist** : les dangereux et les malhonnêtes.
- **Greylist** : les time-wasters (mecs qui veulent juste papoter, négociateurs, lonely guys). Pas dangereux mais coûteux en attention. JUSTYNA les bloque à l'instinct, en quelques secondes.

Pour le profil de volume de JUSTYNA, **pas besoin de calendrier ni de CRM**. Le filtre entrant *est* le produit. Cela précise la séquence de features : triage entrant en priorité 1, mémoire client / agenda à mesure que le volume monte.

## 5. Cheval de Troie multi-plateformes

Andrew valide l'idée de maintenir un profil sur chaque plateforme suisse pour agréger les données et obtenir une porte de distribution. Il accepte la subtilité juridique mais traite la mécanique comme tractable.

## 6. Maia au-delà de Scarlot

Andrew souligne explicitement que le même raisonnement (assistant personnel hébergé par utilisatrice, infra partagée, canal conversationnel natif) s'applique à d'autres segments d'indépendant·es de services. Cohérent avec la vision plateforme Maia déjà documentée. Le travail d'infra investi sur Scarlot sert directement les coverages suivants.

## 7. ProCoRe : signal de douleur, pas concurrent

Tu m'as transmis l'info ProCoRe (sondage sur le besoin de blacklist). Première réaction d'Andrew : "nice". Conclusion partagée après discussion : ça confirme que la douleur est réelle au niveau institutionnel, mais une initiative public-privé dans ce secteur a une espérance de vie courte (mort dès que le budget s'arrête). À traiter comme renforcement de signal, pas comme course au time-to-market.

## 8. Avertissement Cal.com

Andrew cite Cal.com comme contre-modèle : drift vers l'enterprise au détriment de la communauté. Dans cette catégorie, **la communauté est le produit**. Si Maia / Scarlot décolle, il faudra éviter de reproduire ce drift.

## 9. Les métriques qui comptent vraiment

Pas les lignes budgétaires, mais la mécanique de funnel. Andrew valide sans réserve :

- Nombre de pitches par semaine
- Taux de conversion des pitches
- Taux de conversion du bouche-à-oreille

Si le bouche-à-oreille ne prend pas, rien d'autre ne compense.

## Décisions prises pendant le sync

1. **Gel** de tout travail amont sauf le POC.
2. POC sur **Hetzner**, topologie courte durée, pour les 8 premières bêta-utilisatrices. Pas de design d'archi permanente avant validation.
3. Modèle financier : structure conservée autour d'une seule ligne de coût matérielle (salaire co-founder), dev en sweat equity. La ligne représentation est un placeholder à challenger avec toi.
4. Arrangement David : 1% fast money + option jusqu'à 50% du round suivant (au lieu d'un 1% pre-money).

## Ce que j'attends de notre session

- Challenger les paramètres du modèle financier, surtout les placeholders (représentation et autres).
- Pression sur les hypothèses de funnel (pitches/semaine, conversion, bouche-à-oreille) qui portent toute l'unit economics.
- Discuter du seuil minimum de coût runtime par utilisatrice qui rend le modèle viable, à confronter avec ce que le bare-metal partagé peut réellement délivrer.
