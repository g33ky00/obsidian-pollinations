# Pollinations AI for Obsidian

[![CI](https://github.com/g33ky00/obsidian-pollinations/actions/workflows/ci-unified.yml/badge.svg)](https://github.com/g33ky00/obsidian-pollinations/actions/workflows/ci-unified.yml)

> Plugin Obsidian générant du texte et des images avec [Pollinations AI](https://pollinations.ai) directement dans vos notes, en payant avec vos propres Pollen (BYOP — Bring Your Own Pollen).

[![Language](https://img.shields.io/badge/TypeScript-5.6-3178c6?logo=typescript&logoColor=white)](./tsconfig.json)
[![Obsidian](https://img.shields.io/badge/Obsidian-0.15.0+-478bff)](https://obsidian.md)
[![License](https://img.shields.io/badge/License-MIT-blue)](./LICENSE)
[![Quest](https://img.shields.io/badge/Quest-%2315574-blue)](https://github.com/pollinations/pollinations/issues/15574)

## Table des matières

- [À propos](#à-propos)
- [Structure du dépôt](#structure-du-dépôt)
- [Stack technique](#stack-technique)
- [Installation](#installation)
- [Usage](#usage)
- [Sécurité](#sécurité)
- [Contribution](#contribution)
- [Licence](#licence)
- [Changelog](#changelog)
- [Roadmap](#roadmap)

## À propos

- **Quoi** : plugin Obsidian (id `obsidian-pollinations`) permettant de générer du texte à la position du curseur et des images sauvegardées dans le vault, via l'API Pollinations.
- **Pour qui** : utilisateurs d'Obsidian souhaitant intégrer la génération IA dans leurs notes sans quitter l'éditeur.
- **Pourquoi** : éviter la copie-coller depuis des outils externes et garder les générations versionnées dans le vault.
- **Qui** : auteur/mainteneur `g33ky00` ([github.com/g33ky00](https://github.com/g33ky00)).
- **Contacts** : dépôt GitHub [g33ky00/obsidian-pollinations](https://github.com/g33ky00/obsidian-pollinations).

## Structure du dépôt

```text
obsidian-pollinations/
├── .github/
│   └── workflows/
│       └── ci-unified.yml
├── src/
│   ├── image-command.ts
│   ├── model-picker.ts
│   ├── pollinations-client.ts
│   ├── settings.ts
│   └── text-command.ts
├── main.ts
├── manifest.json
├── package.json
├── styles.css
├── tsconfig.json
├── LICENSE
└── README.md
```

## Stack technique

| Composant | Version | Source |
|-----------|---------|--------|
| TypeScript | 5.6.0 | `package.json` (`devDependencies.typescript`) |
| esbuild | 0.24.0 | `package.json` (`devDependencies.esbuild`) |
| Obsidian API | latest | `package.json` (`devDependencies.obsidian`) |
| Node (build) | — | esbuild CLI |
| Plugin | 1.0.0 | `manifest.json` (`version`) |
| Min. Obsidian | 0.15.0 | `manifest.json` (`minAppVersion`) |

## Installation

### Prérequis

- Node.js + npm
- Obsidian ≥ 0.15.0

### Manuel (développement)

```bash
git clone https://github.com/g33ky00/obsidian-pollinations.git
cd obsidian-pollinations
npm install
npm run build
```

Puis dans Obsidian :

1. **Settings → Community Plugins → désactiver le mode restreint** si nécessaire
2. **Settings → Community Plugins → Browse → Load unpacked plugin**
3. Sélectionner le dossier `obsidian-pollinations`

### Release

1. Télécharger la dernière release depuis la [page releases](https://github.com/g33ky00/obsidian-pollinations/releases)
2. Extraire dans `<vault>/.obsidian/plugins/obsidian-pollinations/`
3. Activer dans **Settings → Community Plugins**

## Usage

### Générer du texte

1. Sélectionner du texte ou placer le curseur
2. Lancer la commande « Pollinations: Generate text at cursor » (raccourci par défaut `Ctrl/Cmd + Shift + P`)
3. Le texte généré est inséré à la position du curseur

### Générer une image

1. Sélectionner le prompt image (ou placer le curseur)
2. Lancer « Pollinations: Generate image from selection »
3. L'image est sauvegardée dans le vault et embarquée sous la forme `![[filename.png]]`

### Choisir un modèle

Lancer « Pollinations: Pick model » pour afficher le catalogue de modèles courant (texte/image/audio).

### Authentification (BYOP)

1. Obtenir une clé publique (`pk_...`) depuis [enter.pollinations.ai/keys](https://enter.pollinations.ai/keys)
2. La définir dans **Settings → Plugin Options → Pollinations AI**
3. Lancer « Pollinations: Authenticate with device flow »
4. Visiter l'URL affichée et saisir le code
5. Le jeton d'accès est stocké et utilisé pour les requêtes suivantes

## API Endpoints

| Fonctionnalité | Endpoint |
|---|---|
| Texte (chat) | `POST https://gen.pollinations.ai/v1/chat/completions` |
| Image | `GET https://image.pollinations.ai/{prompt}?model=&width=&height=` |
| Speech | `GET https://gen.pollinations.ai/audio/{prompt}?voice=` |
| Modèles | `GET https://gen.pollinations.ai/{text\|image\|audio}/models` |
| Device flow | `POST https://enter.pollinations.ai/device` |

## Building

```bash
npm install
npm run build
```

Développement avec hot reload :

```bash
npm run dev
```

## Sécurité

- Clés API / jetons stockés dans les settings Obsidian (pas dans le dépôt).
- Aucun secret committé (vérifier `.gitignore`).
- Validation des entrées utilisateur avant appel API.
- Requêtes HTTPS uniquement vers les endpoints Pollinations.

## Contribution

### Conventions de commit

| Préfixe | Usage |
|---------|-------|
| `feat:` | Nouvelle fonctionnalité |
| `fix:` | Correction |
| `docs:` | Documentation |
| `chore:` | Maintenance |

Processus : forker, créer une branche, committer selon les conventions, ouvrir une PR.

## Licence

Ce projet est sous licence **MIT**. Voir [`LICENSE`](./LICENSE).

## Changelog

### v1.0.0 — 2026-11-24

- 📝 README conforme à la charte
- ✅ Plugin initial v1.0.0 (quest #15574)
- ✅ CI unifié (UCQCS)

## Roadmap

### ✅ Terminé

- [x] Plugin initial v1.0.0
- [x] Génération de texte et d'images
- [x] Device flow (BYOP)
- [x] README conforme à la charte

### 🟡 Planifié

- [ ] Publication sur le registre communautaire Obsidian — cible : `manifest.json`
- [ ] Tests automatisés — cible : `src/`

## Demo

### Text generation

1. Open a note or create a new one
2. Type or select a prompt (e.g., "Describe a fantasy tavern")
3. Run **Pollinations: Generate text at cursor** (or use the ribbon icon)
4. The generated text is inserted at the cursor position

### Image generation

1. Select an image prompt (e.g., "A cyberpunk city at night")
2. Run **Pollinations: Generate image from selection**
3. The PNG is saved to your vault and embedded as `![[pollinations_cyberpunk-city_1234567890.png]]`

### Device flow (BYOP)

1. Get a `pk_...` publishable key from [enter.pollinations.ai/keys](https://enter.pollinations.ai/keys)
2. Paste it in **Settings → Plugin Options → Pollinations AI → API Key**
3. Run **Pollinations: Authenticate with device flow**
4. A notice shows the verification URL + code — open it in a browser and approve
5. Your access token is stored and used for all subsequent requests

## Quest

This plugin fulfills [Quest #15574](https://github.com/pollinations/pollinations/issues/15574) — _[QUEST] Pollinations plug-in for Obsidian_

## About

Projet maintenu par g33ky00.

## Contributing

Les contributions sont les bienvenues via pull request.

## License

MIT
