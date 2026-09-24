# Prompt : #retro — Capture RETRO-MODELE

## Déclenchement
Automatique via #bonne-nuit (étape 6) ou manuel à tout moment.

## Protocole strict
1. Lire docs/retro-modele.md — état actuel de la base.
2. Analyser la session :
   - Fichiers modifiés: `git diff --name-only <first-session-commit>..HEAD` (capture entière de session, pas juste dernier commit)
   - Bugs corrigés (.bugdetective/bug-registry.md ou session notes)
   - Décisions architecture (docs/project/decision-log.md)
3. Pour chaque pattern candidat :
   - Chercher un doublon dans retro-modele.md
   - Si similaire existant, ignorer
   - Si nouveau, rédiger au format standard
   - **M5 FIX — Sélection section appropriée**:
     * Bug GAS (closure, Apps Script runtime) → "GAS — Pièges spécifiques"
     * Sécurité (access scope, secrets) → "Sécurité & Robustesse"
     * Déploiement (versioning, quotas) → "Déploiement & Versioning"
     * Performance (optimization) → "Performance & UI"
     * Gouvernance (process, workflow) → "Gouvernance & Scripts"
4. Ajouter dans la section appropriée de retro-modele.md.
5. Rapport final obligatoire :
   "[N] entrée(s) ajoutée(s) : [titres]" ou "Aucune nouvelle entrée détectée"

## Règle d'arbitrage humain

Lorsqu'une décision, un choix d'architecture, une validation humaine ou un GO est demandé :

1. Ne jamais poser uniquement une question fermée sans contexte.
2. Présenter un tableau comparatif avant la question :

| Option | Avantages | Inconvénients | Risques | Impact en l'absence de décision |
| --- | --- | --- | --- | --- |
| A | effets positifs concrets | coûts et limites | risques techniques ou cyber | conséquence opérationnelle |
| B | effets positifs concrets | coûts et limites | risques techniques ou cyber | conséquence opérationnelle |

3. Ajouter les hypothèses et les éléments non vérifiés sous le tableau.
4. Donner un choix recommandé lorsque les preuves permettent de le justifier; sinon écrire explicitement `recommandation impossible : preuve manquante`.
5. Formuler ensuite une seule question de décision, en langage simple, en indiquant l'action automatique possible et la conséquence d'une absence de réponse.
6. Pour une action irréversible ou sensible, rappeler la sauvegarde, le retour arrière et le périmètre exact avant de demander le GO.

Cette règle s'applique aussi aux retours de veille, aux propositions de gouvernance et aux arbitrages entre mécanismes de sécurité. Elle ne transforme jamais une proposition en décision de gouvernance.

## Format entrée standard
- **[Titre pattern] :** [Contexte — 1 phrase].
  [Règle actionnable].
  (*[Projet] — [YYYY-MM-DD]*)

## Sections disponibles
- Gouvernance & Scripts
- GAS — Pièges spécifiques
- Sécurité & Robustesse
- Performance & UI
- Déploiement & Versioning