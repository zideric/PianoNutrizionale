# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Piano Nutrizionale is a **single-file** nutritional meal planning web app for two people (Eric and Chiara). No build process, no dependencies, no server — just open `index.html` in a browser.

## Running the App

```bash
# Open directly in browser (Linux)
xdg-open index.html

# Or with a simple server if needed
python3 -m http.server 8080
```

There are no tests, no linting, and no build steps.

## Architecture

Everything lives in `index.html` (~860 lines): HTML structure, CSS with custom properties, and vanilla JavaScript data + logic.

### Data Layer

Two top-level JavaScript objects defined in the `<script>` tag:

**`data`** — weekly meal plan, keyed by Italian weekday (`lunedi` … `domenica`):
```js
{
  lunedi: {
    colazione: { nome, eric: { titolo, ingredienti }, chiara: { titolo, ingredienti }, nota?, badge? },
    pranzo:    { nome, eric: { ... }, chiara: { ... } },
    cena:      { nome, eric: { ... }, chiara: { ... } }
  },
  ...
}
```

**`ricette`** — recipes keyed by category (`colazioni_dolci`, `colazioni_salate`, `piatti_unici`, `piatti_estivi`, `legumi`, `secondi`, `contorni`). Each recipe:
```js
{
  titolo, fonte, tempo, ingredienti, preparazione, note?, varianti?, stagione
}
```
`fonte` is `'nutrizionista'` or `'creativa'`; `stagione` is `'estate'`, `'inverno'`, or `'tutto'`.

### State & Rendering

Global JS variables track the current view, selected day, person filter, active category, season, and search query. Every user action updates one or more of these variables and calls a render function that rewrites `innerHTML` of the main content area.

Key render functions: `renderMeals()`, `renderTable()`, `renderRecipes()`, `renderRecipeDetail()`.

### Theming

CSS custom properties (`--bg`, `--surface`, `--text`, `--accent`, etc.) drive light/dark mode. The active theme class (`light-theme` / `dark-theme`) is applied to `<body>` and persisted to `localStorage`.

### Companion File

`menu_settimanale_dettagliato.txt` is a plain-text copy of the meal plan data, kept in sync manually for printing/reference. It does **not** drive the app.
