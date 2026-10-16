# GradeOdds

Scan a Pokémon card and see its estimated PSA, BGS and CGC grade, the odds of each grade, recent prices, and whether it's worth paying to submit it.

**Status:** prototype / phase 1. Everything runs in the browser; there is no backend yet.

## What's real and what's sample

| Feature | Status |
|---|---|
| Card catalog (20,635 English cards, through Sept 2026) | Real, from the open [Pokémon TCG dataset](https://github.com/PokemonTCG/pokemon-tcg-data) |
| Card images | Real for 12 bundled cards; placeholder for the rest |
| Centering measurement, auto border detection, photo quality check | Real, computed from the photo's pixels |
| Centering limits per grading company | Real, from each company's published scale |
| Corners / edges / surface scores | Sample numbers (a vision model goes here in phase 2) |
| Prices | Sample numbers (sold-listing data goes here in phase 2) |
| Trends tab: market pulse | Real for the week ending Oct 14, 2026 (Sunday Punch digest); search and pop signals are sample |
| Pro / billing | Demo only, no payment taken |

## Run it locally

It's a static site. Any web server works:

```
python3 -m http.server 8080
```

then open http://localhost:8080. (Opening `index.html` directly from disk works too, but the catalog search needs a server because browsers block `fetch()` on `file://`.)

## Deploy (free)

**GitHub Pages:** already configured. Push to `main` and the workflow in `.github/workflows/pages.yml` publishes the site. Turn it on once under *Settings → Pages → Source: GitHub Actions*.

**Cloudflare Pages / Vercel / Netlify:** connect this repo, build command none, output directory `/`. Point `gradeodds.app` at it in the host's dashboard.

## Install as an app

Open the site on a phone and choose *Add to Home Screen*. The manifest and service worker make it open full-screen and work offline.

## Editing

Everything lives in `index.html`. Near the top of the `<script>`:

- `CARDS` — the four sample cards with hand-written histories and sample prices
- `TIERS` — grading fees and turnaround per company (placeholders; edit to match current price sheets)
- `TRENDS` — the trends tab's signals and verdict notes
- `PSA_CENTER` / `BGS_CENTER` / `CGC_CENTER` — centering limits by grade

To refresh the catalog, rebuild `cards.json` from the dataset repo: each row is `[name, set, number, printedTotal, rarity, artist, releaseDate, id]`.

## Roadmap

1. **Phase 1 (this):** static site, installable, free hosting.
2. **Phase 2:** backend (Supabase free tier), real prices from sold listings, nightly Google Trends job, vision-model grade estimate, perspective correction for tilted photos.
3. **Phase 3:** iOS and Android builds with Capacitor, RevenueCat billing, store listings.

## Not affiliated

GradeOdds is not affiliated with PSA, Beckett, CGC, The Pokémon Company or Nintendo. Grade estimates are a screening tool, not a promise of the grade a company will assign. The Trends tab is speculation, not financial advice.
