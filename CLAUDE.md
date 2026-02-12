# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Winter Olympics 2026 draft game — a zero-dependency, single-file web application tracking Olympic medal counts for a fantasy-style draft competition. 12 competitors each drafted 3 countries; scores are calculated from real medal data fetched from ESPN.

## Development

**No build tools, package manager, or dependencies.** The entire app is a single `index.html` file (HTML + CSS + JS).

- **Run locally:** Open `index.html` in a browser
- **Deploy:** Push to `main` branch; GitHub Pages serves root `index.html` automatically
- **No tests or linting configured**

## Architecture

All code lives in `/index.html`. Key data flow:

1. `fetchMedalData()` — tries multiple CORS proxies to fetch ESPN medal standings HTML
2. ESPN HTML is parsed with `DOMParser`, extracting country names and medal counts into the `medalData` object
3. `findMatchingCountry()` — normalizes ESPN country names to match local draft data (handles variations like "United States" → "USA")
4. `calculateScores()` — applies scoring: Gold=5pts, Silver=3pts, Bronze=2pts, plus Chaos Bonus (+2 per medal for countries outside top 10)
5. `updateLeaderboard()` — ranks competitors by total score across their 3 countries

**Key state:** `draftData` array (hardcoded competitor/country assignments) and `medalData` object (dynamically fetched medal counts).

## Important Considerations

- ESPN HTML parsing is brittle — depends on ESPN's table structure remaining consistent
- CORS proxies (allorigins, corsproxy.io, codetabs) are external dependencies that may go down; the app tries them in sequence as fallbacks
- Country name matching between ESPN and the local draft list requires manual mapping for edge cases
- The `src/index.html` is a duplicate of the root file; keep them in sync or remove one
