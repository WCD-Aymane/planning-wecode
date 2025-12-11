# Documentation Project

## Prérequis

- Python 3.x
- pip ou uv

## Installation

### Avec pip

1. Créer un environnement virtuel :
   ```bash
   python -m venv .venv
   ```

2. Activer l'environnement :
   - Sur macOS/Linux :
     ```bash
     source .venv/bin/activate
     ```
   - Sur Windows :
     ```bash
     .venv\Scripts\activate
     ```

3. Installer les dépendances :
   ```bash
   pip install zensical
   ```

## Lancer le serveur

Pour lancer le serveur de développement et prévisualiser le site :

```bash
zensical serve
```

Le site sera accessible à l'adresse : http://localhost:8000

## Déploiement sur GitHub Pages

Pour publier votre site sur GitHub Pages via GitHub Actions :

1. Créez un fichier `.github/workflows/docs.yml` à la racine du projet.
2. Ajoutez le contenu suivant :

```yaml
name: Documentation
on:
  push:
    branches:
      - master
      - main
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/configure-pages@v5
      - uses: actions/checkout@v5
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: pip install zensical
      - run: zensical build --clean # (1)!
      - uses: actions/upload-pages-artifact@v4
        with:
          path: site
      - uses: actions/deploy-pages@v4
        id: deployment
```

3. Poussez vos changements sur la branche main/master.
4. Votre site sera accessible à l'adresse `https://<username>.github.io/<repository>/`.
