# Mood Machine — marketing site

Landing page for **Mood Machine**: a privacy-first way to build the right playlist for the moment, the place, and the people in the room — drawn from your own music library, by voice or text.

**Live:** https://brave-meadow-0b0660410.7.azurestaticapps.net/

## What this repo is

A single, self-contained, fully responsive `index.html`. All styles, scripts, and fonts are bundled in, so there is no build step. The page was designed in Claude and exported as a bundled page.

## Tech

- Plain static HTML / CSS / JS — no framework, no build
- Hosted on **Azure Static Web Apps**
- **Auto-deploys** on every push to `main` via GitHub Actions

## Deploy

Commit to `main` and you're done. The workflow in `.github/workflows/` publishes to Azure automatically; the site is live about a minute later. To update the page, replace `index.html` and commit.

## Files

- `index.html` — the entire site (markup, styles, scripts, fonts)
- `staticwebapp.config.json` — Azure Static Web Apps routing and headers
- `.github/workflows/` — CI/CD to Azure Static Web Apps

---

© 2026 Mood Machine · Private by design · Works with Spotify
