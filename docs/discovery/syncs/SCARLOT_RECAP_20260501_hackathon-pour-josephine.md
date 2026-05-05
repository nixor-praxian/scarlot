---
date: 2026-05-01
pour: Joséphine
sujet: récap du hackathon Scarlot avec Andrew (Day 1, 30 avril 2026)
sources:
  - SCARLOT_SYNC_20260430_{0900_hackathon-day1-morning,1316_hackathon-day1-noon-david,1443_hackathon-day1-noon-harness,1818_hackathon-day1-afternoon-devops}.md
  - Project Recap - 2026-05-01.md (hernest)
  - Website Overview - 2026-05-01.md (scarlot-website)
  - Safety Data Recap - 2026-05-01.md (scarlot-safety-data)
---
Salut Joséphine. Voici un récapitulatif de ce qu'on a fait avec Andrew ; ce qui est utilisable aujourd'hui, ce qui ne l'est pas encore. Malgré le petit coup de stress avec Papa, c'était une super expérience de faire ça avec Andrew et mine de rien, on a avancé sacrément vite. 

J'ai essayé d'éviter le jargon. Si quelque chose n'est pas clair, n'hésite pas à poser la question. 

**En résumé** : le système est "complet" bout en bout : un user peut s'inscrire sur le site, recevoir un QR code, scanner WhatsApp, et commencer à parler à Scarlot dans son chat admin. 

Sur le site, il y a 2 propositions de "chemins" : 

1. "Mode contact" - comme si tu écrivais à une tierce personne dans WhatsApp | **NON FONCTIONNEL**
	1. le user enregistre dans ses contacts et lui écrit. Elle décide quand lui transférer un message ou un contact. Mode le moins intrusif, le plus simple à essayer.
2. "Mode Intégré" - comme si tu écrivais à toi-même dans WhatsApp | **FONCTIONNEL**
	1. le user paire son WhatsApp à Scarlot une fois, comme on paire WhatsApp Web. Ensuite elle écrit à elle-même (note personnelle), et Scarlot lui répond dans ce fil. Mode plus puissant, qui demande plus de confiance.

*Je reviens plus tard sur les différences intrinsèques de chaque approche (et sur une potentielle 3ème approche future dont on devra discuter / tester pendant les interviews / user testing).* 

**Il faut donc tester le mode Intégré uniquement à ce stade.** 

Une chose à savoir : la fonction safety (le lookup d'un numéro de téléphone contre la base communautaire / grey list / blacklist) n'est pas encore reliée à Scarlot. Pour l'instant les users peuvent définir leurs propres numéros blacklistés pour eux-mêmes. 

**Tech - Services**
Le système est découpé en trois services distincts. Cette séparation a une raison : chaque service ne connaît que ce qui lui est nécessaire pour faire son travail. Si l'un est compromis, les autres ne sont pas touchés.

### Le site 

C'est la porte d'entrée. Un user potentiel arrive, entre son numéro, reçoit un QR code à scanner sur WhatsApp pour terminer l'installation (pour le mode 2). Le site est en quatre langues (français, anglais, allemand, espagnol) avec détection automatique. Il devine le pays du téléphone par l'IP. Il transmet le numéro au système de provisioning (comprendre ici, l'autre service qui crée un container par user, c'est-à-dire "un espace personnel" sur le serveur) et redirige vers la page QR. 

Évidemment, ne t'arrête pas sur le copywriting ou le design, on a fait ça en 5min. Notre but était plus d'avoir le système end-to-end. On va polir tout ça ensemble comme il faut. 

### L'assistant

C'est le produit que l'utilisateur utilise. Un agent qui vit dans son chat WhatsApp et qui répond en français (ou allemand, italien, anglais) à des messages en langage naturel.

L'utilisateur ouvre WhatsApp et écrit comme à un-e ami-e. Quelques exemples qui marchent déjà :

- "J'ai vu Marc hier soir, il a payé 300. Rappelle-moi son prochain rendez-vous?"
- "Bloque le numéro qui m'a envoyé ça."
- "Qu'est-ce que tu sais sur ce numéro ?"
- "Quel est mon programme de demain ?"
- "Ajoute Sofia, premier rendez-vous, elle paye en cash."

**Je reviens sur les 2 modes...** dans les deux cas, Scarlot ne lit jamais les conversations avec les clients et ne répond jamais à leur place. Cette règle est implémentée dans le code, pour qu'aucune dérive ne soit possible. Mais il y a une valeur à terme d'avoir une cohorte de users qui nous donneront accès à leurs conversations. On en reparlera, mais c'est sacrément touchy. 

Les actions importantes (ex. mettre quelqu'un sur la blacklist, supprimer un client) ne s'exécutent pas sans une confirmation explicite. Scarlot pose la question et attend un "oui" sur un deuxième message. Il y a tout un point à évaluer sur l'expérience, on va y revenir. 
C'est là à mon sens où se trouve la véritable innovation de ce qu'on est en train de réaliser. Faut que ce soit super fluide et pas comme un "bête" chatbot. 

### La base de réputation 

C'est le service qui répond à la question "ce numéro est-il connu pour être problématique ?". Au hackathon on l'a appelé le Cheval de Troie : il aspire les données des plateformes existantes (And6 d'abord, d'autres ensuite) pour les mutualiser dans une seule réponse.

Côté données : And6 est entièrement aspiré, soit 10'228 signalements répartis sur 7'876 numéros de téléphone, depuis 2012. Pour chaque numéro, le service peut répondre : statut (`blacklist`, `greylist`, `clean`, ou `unknown`), catégories de problème (cinq publiques : `time_waster`, `no_show`, `abusive`, `scammer`, `dangerous`), nombre de signalements, premier et dernier signalement, et un résumé d'une ligne. 

Côté IA : un modèle local (qui tourne sur mon serveur à la maison) a relu l'ensemble de la base en complément des règles déterministes. Avec lui seul, on laissait 51,9% des signalements en "unknown" parce que les commentaires sont en cinq langues mélangées et le vocabulaire bouge. Avec les deux ensemble, on tombe à 3,2%. L'IA a aussi identifié indépendamment une catégorie qu'on avait sous-estimée (`health_risk`, pour les signalements qui vont en ce sens) qui représente 2,8% du corpus.

Ce qui n'est pas encore fait sur ce service : 
* la possibilité d'être appelé par "L'Assistant" ; c'est le travail qui débloque la connexion entre Scarlot et la base de données
* l'accès à TOUTES les sources d'informations liées à la sécurité pour avoir une couverture maximale et régler un des plus gros pain points de nos users en centralisant cette information cruciale 

## 3. Ce qu'un user peut faire aujourd'hui

| Capacité                                                      | Mode intégré     |
| ------------------------------------------------------------- | ---------------- |
| Enregistrer un nouveau client en parlant à Scarlot            | Oui              |
| Tenir une fiche par client (pseudo, notes, statut, paiements) | Oui              |
| Logger un rendez-vous                                         | Oui              |
| Logger un paiement, demander qui n'a pas payé ce mois-ci      | Oui              |
| Bloquer / débloquer un numéro avec confirmation               | Oui              |
| Demander en français, allemand, italien ou anglais            | Oui              |
| Envoyer un audio (vocal) au lieu de taper                     | Pas encore       |
| Demander si un numéro est signalé ailleurs                    | Pas encore relié |

La ligne qui manque le plus, et qui est la priorité numéro un, est la fonction safety lookup partagée.

---

## 4. Ce qui marche aujourd'hui

Cette liste est vérifiée dans le code et a tourné pendant le hackathon.

- L'inscription bout en bout : numéro sur le site, QR code, pairing WhatsApp, premier message à Scarlot.
- Le déploiement multi-tenant. Un container par TDS, isolé des autres.
- Les 17 outils côté Scarlot (créer, chercher, mettre à jour les fiches client ; rendez-vous ; paiements ; blacklist ; paramètres).
- Le passage par "porte cheap" avant l'IA : les commandes simples (`/help`, `/status`, un "oui" de confirmation) sont traitées sans appel à l'IA. Économie de coût, et plus déterministe.
- Le masquage des numéros : l'IA ne voit jamais un numéro brut. Elle voit `PHONE_1`, `PHONE_2`, etc. Le code traduit en numéros réels au moment de toucher la base. Cela protège les numéros et limite les risques de manipulation par message piégé.
- Le rappel automatique avant un rendez-vous (vérifié toutes les 60 secondes).
- Le journal d'événements : chaque modification est enregistrée, on peut retracer ce qui s'est passé.
- L'aspiration complète de And6
	- L'enrichissement IA local sur cette base (taux d'inconnu de 51,9% à 3,2%).

---

## 5. Ce qui n'est pas encore fait

**La connexion entre Scarlot et la base de réputation.** C'est le point le plus important à terminer avant d'ouvrir le pilote. Le code de Scarlot prévoit l'appel, le contrat d'API est figé, la base existe et répond, mais le service multi-tenant côté safety-data (Phase 5 du plan) n'est pas commencé. Tant que ce travail n'est pas fait, la fonction "ce numéro est-il signalé ?" n'est pas utilisable depuis WhatsApp.

**Le chiffrement au repos.** Aujourd'hui les données des TDS sont en clair sur le disque du serveur. Le design est fait (SQLCipher avec une clé par TDS), mais le code n'est pas écrit. Le modèle de confiance actuel est "fais confiance à l'opérateur". C'est acceptable pour un cercle pilote restreint où tu nous connais et où on connaît les testeuses, pas au-delà.

**Aucun user réel.** Le système est branché mais personne ne l'a encore utilisé. Tout ce qu'on dit sur "ça marche" est vrai au sens technique, pas au sens vécu.

**Le vocal.** On avait souligné l'importance des notes vocales WhatsApp. Ce n'est pas branché aujourd'hui. La transcription via Gemini Flash est validée techniquement et ce n'est pas un gros travail à câbler. 

**Pas de vérification SMS / OTP sur le site.** N'importe qui peut entrer un numéro qui n'est pas le sien et déclencher un provisioning. Acceptable pour un pilote de personnes qu'on connaît, pas pour une ouverture publique.

**Pas de rate limiting sur l'inscription.** Quelqu'un de mal intentionné pourrait saturer le système et créer beaucoup de tenants vides. Petit travail à ajouter.

**Pas de soft-delete, pas de backups automatiques, pas de SLA.** Si une testeuse demande "supprime tout sur moi", on doit le faire à la main. Si une base est corrompue, on n'a pas encore de procédure de restore.

**Le DPIA n'est pas finalisé.** Il y a encore des questions ouvertes dans le brouillon. À terminer avant qu'un consommateur externe soit branché à la base de réputation.

**Une seule source de signalements.** And6 est solide à 10k records, mais une seule source ne suffit pas pour bien calibrer le facteur de diversité dans le score de confiance. 

**L'hébergement n'est pas en Suisse.** On a tranché pour un fournisseur allemand au stade pilote pour une raison de prix et de rapidité. C'est un sujet à revisiter avant le "passage en production" : le stockage en Suisse pourrait avoir une vraie valeur perçue avec la communauté et probablement une valeur légale pour le nFADP (à valider).

---

## 6. Ce sur quoi j'ai besoin de toi avant qu'on lance le pilote

**La proposition de valeur, tone of voice, branding et l'expérience avec l'Assistant.**

**La taxonomie publique des catégories de signalement.** Aujourd'hui on expose cinq catégories (`time_waster`, `no_show`, `abusive`, `scammer`, `dangerous`), plus une sixième en interne (`health_risk`) qu'on pourrait promouvoir publiquement. Question : est-ce que `health_risk` doit apparaître comme catégorie distincte dans la réponse à une TDS, ou est-ce que ça rentre dans `dangerous` ? À toi de trancher, c'est un choix de communauté plus que de produit.

**Les users beta :** Tu avais une première liste en tête. Le système est prêt à les accueillir une par une. Idéalement un mélange de profils (volume, niveau technique, persona). On va en parler plus en détail mais on peut déjà préparer une liste.

## 7. Notes des conversations du hackathon

Quelques décisions qui ne demandent pas d'action de ta part mais qui sont utiles à connaître.

**Conversation cadrante avec David à midi.** Il nous a fait reprendre la description du produit à zéro. Trois choses utiles ressortent. D'abord, Scarlot n'est pas un canal, c'est une couche au-dessus des canaux : Discovery (les sites d'annonces) reste en amont, le rendez-vous lui-même reste en aval, on couvre uniquement le messaging entre les deux. Ensuite, les rapports (white/grey/blacklist) pourraient être uniquement des tags (par exemple des emojis), zéro texte libre, pour des raisons légales (zéro surface de diffamation) et alignées avec l'anonymat de la communauté. Enfin, le produit "réputation" et le produit "CRM" (notes, contacts, calendrier, tracking des finances) sont deux POCs séparables avec des dynamiques différentes ; le mettre en deux services nous donne plus de souplesse pour tester chaque willingness-to-pay séparément.


---

## 8. Trajectoire proposée pour les deux prochaines semaines

Voici l'ordre qui me semble juste, à valider avec toi.

1. terminer la connexion Scarlot vers la base de réputation. C'est ce qui débloque la fonction la plus importante
2. avoir accès à plus de plateformes de rapport pour étoffer notre base de donnée safety 
3. mettre en place une automatisation pour toujours avoir la base de donnée safety à jour
4. brancher le vocal. Pas critique mais facile à câbler
5. Avant les premiers users : 
	1. Tester à mort nous-mêmes 
	2. valider le document d'information pilote et les questionnaires d'interview, je le branche dans le flow d'inscription
6. Les `n` premiers users, individuellement, avec un canal de feedback direct (probablement un chat WhatsApp commun avec toi et moi en backend)
7. Avant tout déploiement plus large : plus de sécurisation, archivage et sauvegarde etc. 

Voilà, sorry pour le long mail... Je pense avoir couvert la majorité du sujet :) 

---

## Une dernière note

C'était un vrai bonheur de travailler avec Andrew sur ce hackathon. Au-delà du résultat technique, l'expérience humaine était top : on a beaucoup avancé, beaucoup ri, beaucoup appris l'un de l'autre. Le genre de session de boulot dont on se souvient. 

To infinity and beyond! 🚀

Bien à toi, 