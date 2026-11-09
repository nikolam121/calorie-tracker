# Calorie Tracker

A web app for logging food and tracking calorie and macronutrient intake. Search a food database, log portions by weight, and see running totals for calories, protein, fat, and carbohydrates.

Live demo: https://nikolam121.github.io/calorie-tracker/

## Features

- **Food search with autocomplete**, type a food name and pick from live suggestions backed by the Edamam food database. Requests are debounced so typing doesn't fire one call per keystroke.
- **Portion-based logging**, enter an amount in grams; macros are calculated from the per-100g values returned by the API.
- **Live totals**, calories, protein, fat, and carbs update as entries are added or removed.
- **Works without an account**, the tracker is fully usable logged out (entries are kept in the browser's local storage). Logging in carries those entries over to your account rather than discarding them.
- **Saved history**, signed-in users have their entries stored per-user in Firebase Firestore and synced live across tabs.
- **Responsive**, single-column layout on mobile, no horizontal scrolling at any width.

## Tech stack

| Layer | Technology |
| --- | --- |
| Front end | HTML, CSS, vanilla JavaScript (ES modules) |
| Auth | Firebase Authentication (email/password) |
| Database | Firebase Firestore |
| Food data | Edamam Food Database API |

## Running locally

The app uses ES modules, so it needs to be served over HTTP rather than opened directly from the filesystem.

```bash
git clone https://github.com/nikolam121/calorie-tracker.git
cd calorie-tracker
cp config.example.js config.js   # then fill in your Firebase and Edamam keys
python -m http.server 8000
```

Then open `http://localhost:8000`.

## Configuration

Firebase and Edamam credentials live in `config.js`, which is not committed (it is listed in `.gitignore`). Copy `config.example.js` to `config.js` and fill in your own values.

Because this is a static client-side app with no backend, the keys in `config.js` are still visible to anyone using the running app. The Firebase web API key is designed to be public: access is controlled by Firestore security rules, not by hiding the key. The Edamam key is not, so keep it on a free-tier plan and replace it if the quota starts getting used up. Moving the food lookups behind a small backend proxy is the proper fix if this ever needs to scale.

## Deployment

The site is published to GitHub Pages by the workflow in `.github/workflows/deploy.yml`. On every push to `master` it writes `config.js` from the `CONFIG_JS` repository secret (Settings > Secrets and variables > Actions) and publishes the static files, so the keys are not stored in the repository.

## Project structure

```
index.html          Tracker page
main.js             Food search, entry state, totals, Firestore sync
main.css            Shared styles and design tokens
auth.html           Sign up / log in page
auth.js             Firebase Authentication flows
auth.css            Auth page layout
config.example.js   Template for config.js (Firebase and Edamam keys)
```
