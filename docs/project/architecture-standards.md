# Architecture Standards — Book Nils Beneville

## Stack
- **Runtime** : Static HTML/CSS/JS (aucun framework)
- **Hébergement cible** : GitHub Pages
- **Fonts** : Google Fonts (Playfair Display, Inter)

## Structure
```
/
├── index.html          # Page unique
├── Photos/             # Assets images
├── .github/            # Gouvernance Copilot
└── docs/               # Documentation projet
```

## Patterns
- Single page, navigation par ancres
- Responsive (mobile-first breakpoints : 480px, 768px)
- Animations CSS + IntersectionObserver
- Aucun bundler, aucune dépendance NPM

## Déploiement
- GitHub Pages depuis branche `main`
- Pas de build step — fichiers servis directement

## Sécurité
- Pas de backend, pas de données utilisateur
- Email exposé via lien Gmail compose (pas de formulaire côté serveur)
- Aucun secret dans le code
