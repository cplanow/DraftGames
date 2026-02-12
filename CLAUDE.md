# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Winter Olympics 2026 draft game — a zero-dependency, single-file web application tracking Olympic medal counts for a fantasy-style draft competition. 12 competitors ("managers") each drafted 3 countries; scores are calculated from real medal data fetched from Wikipedia.

## Development

**No build tools, package manager, or dependencies.** The entire app is a single `index.html` file (HTML + CSS + JS).

- **Run locally:** Open `index.html` in a browser
- **Deploy:** Push to `main` branch; GitHub Pages serves root `index.html` automatically
- **No tests or linting configured**

## Scoring Rules

**Medal points:** Gold = 5pts, Silver = 3pts, Bronze = 2pts

**Chaos Modifier:** At the end of the Games, identify the final top 10 countries on the official medal table (by the rank shown on that table — gold first, then silver, then bronze). For any medal earned by a drafted country finishing outside the top 10, add +2 bonus points per medal.

**Manager total score** = sum of all points from their 3 drafted countries (medal points + chaos bonuses).

**Tiebreakers (in order):** Most gold medals across drafted countries → most total medals → most chaos medals → single-best day score → coin flip. (Not yet implemented in code.)

## Architecture

All code lives in `/index.html`. Key data flow:

1. `fetchMedalData()` — tries multiple CORS proxies to fetch Wikipedia medal table HTML
2. Wikipedia HTML is parsed with `DOMParser`, extracting country names from `<th scope="row">` cells and medal counts from `<td>` cells into the `medalData` object
3. `findMatchingCountry()` — normalizes Wikipedia country names to match local draft data (handles variations like "United States" → "USA", "Czechia" → "Czech Republic")
4. `calculateScores()` — ranks countries by gold-first (standard Olympic ranking) to determine top 10, then applies medal points + chaos bonus
5. `updateLeaderboard()` — ranks competitors by total score across their 3 countries

**Key state:** `draftData` array (hardcoded competitor/country assignments) and `medalData` object (dynamically fetched medal counts).

## Important Considerations

- CORS proxies (allorigins, corsproxy.io, codetabs) are external dependencies that may go down; the app tries them in sequence as fallbacks
- Country name matching between Wikipedia and the local draft list requires manual mapping for edge cases in `findMatchingCountry()`
- The `src/index.html` is a duplicate of the root file; keep them in sync or remove one
