# Orbit

Orbit is a ten-question space quiz with instant answer feedback, educational
facts, mission progress, and a final score. Review your answers after finishing,
then replay in the original order or with shuffled questions.

The responsive interface includes light and dark themes, keyboard-friendly
controls, accessible feedback, reduced-motion support, and an animation toggle.
Everything lives in `index.html`: HTML, CSS, JavaScript, and SVG artwork. There
are no external assets, dependencies, or build tools.

## Run locally

Clone this repository and open `index.html` in any modern browser. On Windows,
run this command from the repository directory:

```powershell
Start-Process .\index.html
```

The quiz works directly over `file://`, without a server or network access.
Scores are kept in memory and reset when the page reloads.

## Live website

[Play Orbit on GitHub Pages](https://jamont3089.github.io/space-quiz-bend/)

## Deployment

The [Pages workflow](.github/workflows/pages.yml) copies `index.html` into a
site-only artifact; no compilation is needed. Pull requests targeting `main`
prepare and upload the artifact without deploying. Pushes to `main` deploy the
artifact to GitHub Pages. The workflow can also be run manually from the Actions
tab; deployment is restricted to `main`.

In repository **Settings > Pages**, the build and deployment source must be
**GitHub Actions**. Deployment uses the `github-pages` environment and GitHub's
short-lived deployment token, so no repository secrets are required.
