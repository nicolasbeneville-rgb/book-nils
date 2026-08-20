# Clasp Project Registry

Source de verite pour le compte Google attendu sur chaque projet Apps Script.

## Regles
- `PRO` = compte Google professionnel requis pour toutes les commandes `clasp` du projet.
- `PERSO` = compte Google personnel requis pour toutes les commandes `clasp` du projet.
- `TO_CONFIRM` = compte non arbitre, aucune commande `clasp push` ou `clasp deploy` sans confirmation prealable.

## Matrice projets
| Projet | Compte clasp attendu | Statut |
| --- | --- | --- |
| A3_Stocks | PRO - nicolas.beneville@veolia.com | Confirme |
| bgcampus-presentation | PERSO - nicobeneville@gmail.com | Confirme |
| Book_Nils | PERSO - nicobeneville@gmail.com | Confirme |
| CleanUp_Temp | PERSO - nicobeneville@gmail.com | Confirme |
| Perso_CleanUp | PERSO - nicobeneville@gmail.com | Confirme |
| Webapp_Audit360 | PRO - nicolas.beneville@veolia.com | Confirme |
| Webapp_Digitools | PRO - nicolas.beneville@veolia.com | Confirme |
| Webapp_Gestion_Club | PERSO - nicobeneville@gmail.com | Confirme |
| Webapp_Harmonisation | PERSO - nicobeneville@gmail.com | Confirme |
| Webapp_ImportToVams | PRO - nicolas.beneville@veolia.com | Confirme |
| Webapp_Nettoyage_Mail | PRO - nicolas.beneville@veolia.com | Confirme |
| Webapp_Onboarding | PRO - nicolas.beneville@veolia.com | Confirme |
| Webapp_Pilotage_Contrat | PRO - nicolas.beneville@veolia.com | Confirme |
| Webapp_Processus_LM | PERSO - nicobeneville@gmail.com | Confirme |
| Webapp_Processus_Vivao | PRO - nicolas.beneville@veolia.com | Confirme |

## Ancrage local par projet
Chaque sous-projet doit reporter cette information a 2 endroits :
- `.github/copilot-instructions.md`
- `docs/project/operating-rules.md`

Format recommande :
- `Declared clasp account: <PRO|PERSO|TO_CONFIRM> - <email ou alias exact>`
- `Any push, version, or deploy with another account is blocked.`
