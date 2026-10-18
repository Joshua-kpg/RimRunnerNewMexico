# RimRunnerNewMexico

Unofficial New Mexico HS boys basketball ratings. Not affiliated with the NMAA or MaxPreps.
Status: **HELD - waiting for hosting.** Planned home: https://rimrunnernewmexico.web.app/ (Firebase Hosting). The Firebase project could not be
created yet (Google project quota on the hosting account) and Cloudflare Pages is at its project limit. Working preview: this repo's GitHub Pages copy
(https://joshua-kpg.github.io/RimRunnerNewMexico/). Until the site's Firebase project exists, accounts, picks and follows save only in the visitor's browser.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Teams
151 teams: the NMAA 2026-2028 boys basketball classification and alignment (Section IV, revised Oct 16 2026): 5A 25, 4A 29, 3A 27, 2A 32,
1A 37 (NMAA's Class A) in 28 NMAA districts (NMSD is an NMAA Class A independent), plus 1 Other New Mexico school (Clovis Christian, whose boys
are not on the NMAA boys alignment) that played 8+ NMAA teams in 2025-26.

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 06:30 and 14:00 UTC
(11:30 pm / 7:00 am Mountain standard time) and on manual dispatch. Before the New Mexico boys season opens
(`SEASON_START` in index.html = the first game on the MaxPreps schedules, on or after the NMAA first allowed contest day, Sat Nov 21, 2026)
the script makes no requests. From then on it reads boys basketball finals for the last 3 days from the MaxPreps New Mexico scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and bakes scores and W-L
records into `index.html`. The page recomputes Elo from those finals.
It commits only when data changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
