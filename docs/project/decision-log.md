# Decision Log — Book Nils Beneville

## Entry Format
### YYYY-MM-DD - Title
- Decision: what was decided
- Rationale: why
- Impact: scope and consequence
- Owner: who owns the decision

---

### 2026-05-30 - Project kickoff et choix techniques
- Decision: page HTML unique statique, hébergement GitHub Pages, style cinématographique sombre.
- Rationale: simplicité maximale pour un book d'acteur, pas besoin de backend.
- Impact: aucune dépendance, maintenance quasi nulle.
- Owner: Nicolas Beneville.

### 2026-05-30 - CTA via Gmail compose
- Decision: utiliser un lien Gmail compose au lieu de mailto.
- Rationale: mailto ne fonctionne pas sur tous les navigateurs sans client mail configuré.
- Impact: dépendance à Gmail, mais couvre 90% des cas d'usage.
- Owner: Nicolas Beneville.

---

## Apprentissages rétrospectivement documentés

### 2026-06-15 - 💡 [RETRO-MODELE] Gouvernance Copilot standardisée
- Decision: importer la couche .github (agents, skills) et docs de gouvernance depuis le modèle centralisé pour tous les projets.
- Rationale: standardiser le pilotage, la sécurité et la traçabilité; hériter des guardrails et bonnes pratiques.
- Impact: tous les projets bénéficient des mêmes règles de déploiement, security et documentation; maintenance centralisée.
- Owner: Architecture.
- Source: pattern appliqué dans tous les projets (2026-04-24 onwards).

### 2026-06-15 - 💡 [RETRO-MODELE] Fallback UserProperties pour erreurs Sheet
- Decision: implémenter fallback sur UserProperties quand accès Sheet refusé.
- Rationale: conserver l'état sans droit d'écriture Sheet.
- Impact: résilience accrue.
- Owner: Architecture Apps Script.
- Source: pattern Webapp_Digitools (2026-04-27).
