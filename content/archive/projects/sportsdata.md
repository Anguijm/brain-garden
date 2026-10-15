---
type: project-record
title: "sportsdata: the record of a sports prediction platform, 2026-04 to 2026-07"
status: curated
draft: true
archived: 2026-10-15
created: 2026-10-15
tags: [projects, sportsdata, prediction, harness-cli, record]
---

<!-- Promoted 2026-10-15 from 10_staging/sportsdata-record/synthesis.md (kept there). The
     operator's ruling: document the research and the findings, then kill the repo. draft: true
     keeps it off the public garden until he decides; flip draft to false to publish. -->

# sportsdata: the record of what it was, what it found, and why it was retired

This note is the only record of the sportsdata project that survives the repository. The project was a US sports data platform that scraped six leagues into SQLite, predicted game winners and point spreads with a hand-built sigmoid model, and spent two months trying and failing to beat that simple model with machine-learned models under a strict, pre-declared review process. The one durable finding is negative and well measured: on about four thousand NBA games, nothing the project built beat a logistic curve on season-to-date point differential, and five separate attempts said so with confidence intervals.

Labels follow the vault's rules. FACT names its source in the same sentence. Numbers that this session computed by querying the final database dump are labeled FACT with the query named, and what they mean is labeled Assessment. Expanded acronyms on first use: API (application programming interface), MAE (mean absolute error), ECE (expected calibration error), CI (confidence interval), ATS (against the spread), EWMA (exponentially weighted moving average), BPM (Box Plus/Minus), AUC (area under the receiver operating characteristic curve), TOV (turnovers), bbref (basketball-reference.com), PR (pull request), SQL (structured query language), CV (cross-validation).

## 1. What the project was and when

The project lived for about eight weeks of real work in April and May 2026, then ran unattended until it was retired on 2026-10-15.

FACT: the first commit on `main` is "Initial scaffold: sports data platform with council governance and ratchet loop", dated 2026-04-04 19:24 JST, per `git log origin/main --reverse` in the laptop clone. FACT: `main` holds 226 commits and the repository reached pull request number 91, per `git log origin/main | wc -l` and `gh pr list --state all`. FACT: the last change to models or scrapers was 2026-05-29 (PR #87 "canonical_id on games" and PR #88), and the last three commits of any kind were harness maintenance on 2026-07-08 (PR #89), 2026-07-10 (PR #90, "bump retired gemini-2.5-pro to gemini-3.5-flash") and 2026-07-10 22:11 UTC (PR #91, "Adopt harness-cli V0.3.2"), per `git log origin/main --format='%ad %s' --date=iso`. FACT: the repository description reads "US sports data analytics platform with automated scraping, council-governed analysis, and Jon Bois-inspired visualization", per `README.md` line 3.

FACT: the founding work order (`MEMORY/WORK/20260404-100943_sportsdata-platform-research-council/PRD.md`) states the intent as combining three patterns: Karpathy's "AutoResearch" ratchet loop (hypothesize, modify, execute, evaluate, keep or revert), the yolo-projects council and evaluation gates, and the Roadtripper `harness.yml` council configuration; the stack was TypeScript and Node with Python for machine learning. FACT: the same file records the risk list written on day one, including "Prediction accuracy requires significant historical data before being meaningful" and "Council review overhead could slow iteration speed".

The sprint timeline, from `SESSION_LOG.md` and `SESSION_HANDOFF.md` (FACT for each date; the subagent extraction cites `SESSION_LOG.md` lines 317 to 1017 and `SESSION_HANDOFF.md` lines 7 to 207):

- 2026-04-04: Sprints 1 to 3. Scaffold, SQLite store, scheduler, the Odds API client, `game_results` with auto-resolution, 3,871 NBA games from BallDontLie.
- 2026-04-05: Sprints 4 to 6. The "Interesting Things" detector and frontend, player stats for six sports (5,049 players), and the first ratchet loop run on NBA.
- 2026-04-06 to 04-09: Sprints 7 to 10.5. Live predictions with a twice-daily cron, Fly.io and Cloudflare deploys, a resolver, a calibration plot, and the discovery that both the Cloudflare deploy and the scrape cron had been failing silently.
- 2026-04-09 to 04-14: Sprint 10.6 and 10.7. The scrape route repaired, the v4-spread margin model, the v5 continuous sigmoid, a 12,813-prediction historical backfill, and the injury signal.
- 2026-04-14 to 04-22: Sprints 10.8 to 10.10. Per-sport baselines with bootstrap CIs, the soccer Poisson null result, reliability diagrams, the NBA home-advantage recalibration, MLS and EPL sigmoid rescaling, and shadow-prediction logging.
- 2026-04-24 to 04-30: Sprints 10.11 to 10.22. The NBA learned-model program: Phase 1 (premise failed), Phase 2 (box-score data, shipped), Phase 3 (LightGBM and MLP, null), Phases 4 to 6 (BPM prior, bug fixes, season net rating, all null), then a debt sweep and the automated Gemini council.
- 2026-05-01 to 05-24: Sprints 10.23 to 10.25. Phase 7 (hybrid season-aggregate plus EWMA-delta): a 2021 and 2022 backfill, two latent pipeline bugs found and fixed, and a null result on the validation fold.
- 2026-05-28 and 05-29: an injury scraper fix (PR #85) and the canonical game id (PRs #87, #88).
- 2026-07-08 to 07-10: harness-cli adoption. Nothing after.

Assessment: the project ended when the learned-model program ran out of hypotheses that fit the data it had. `SESSION_HANDOFF.md` lines 24 to 34 close with "debt #37: a strategic decision on Phase 8 direction needs user-level input before any Phase 8 plan PR", and that input never came.

## 2. How it worked

The system was a scraper feeding a SQLite database on a Fly.io machine, a set of hand-written prediction functions that ran twice a day from a GitHub Actions cron, a static frontend on Cloudflare Pages, and a layer of review process on top of every change.

### 2.1 Data sources and the scrape

FACT: the sources were the undocumented ESPN API for scores, teams, schedules, box scores, injuries and probable pitchers (no key, rate limited at 60 requests per minute in `harness.yml`); The Odds API for betting lines (500 requests per month free); BallDontLie for NBA historical games (5 requests per minute free); and basketball-reference.com scraped at one request per 30 seconds for audits only, per `README.md` lines 36 to 44 and `harness.yml` lines 68 to 85. FACT: SportsData.io and API-Football were listed as "Planned" and never built, per `README.md` lines 43 to 44.

FACT: a scrape cycle fetched each league's teams and the scoreboard for each day in a rolling window (today plus three prior days by default, up to 14), fetched odds if a key was set, wrote odds onto games, and resolved outcomes for games that had gone final, per `DEPLOY.md` section "Manually trigger a scrape + resolver sweep". FACT: the production cron (`.github/workflows/predict-cron.yml`) posted `/api/trigger/scrape?sport=all` and then `/api/trigger/predict?sport=all` to the Fly app with a bearer token at 05:00 and 22:00 UTC. FACT: every row carries a `provenance_json` column recording the source, the URL and the retrieval time, per the `CREATE TABLE teams` statement in the 2026-10-14 dump.

### 2.2 Storage and schema

FACT: the final database, from the dump `sportsdata.sql.gz` in the GitHub release `backup-2026-10-14`, has thirteen tables: `teams`, `games`, `odds_raw`, `game_results`, `team_mappings`, `player_stats`, `predictions`, `player_injuries`, `nba_game_box_stats`, `nba_box_stats_audit`, `scrape_warnings`, `nba_espn_event_ids` and `nba_neutral_site_games`, per `zcat sportsdata.sql.gz | grep "^CREATE TABLE"`. FACT: it loads to a 64,036,864-byte SQLite file, per `ls -la` after `zcat ... | sqlite3 sd.db` in this session.

FACT: the row counts in that dump are 24,774 games, 24,563 game results, 14,351 predictions, 7,604 NBA box-score team-rows, 20,024 box-score audit rows, 5,049 player stats, 3,802 ESPN event id mappings, 1,343 injuries, 300 raw odds responses, 178 teams, 90 team mappings, 11 scrape warnings and 6 neutral-site games, per `zcat sportsdata.sql.gz | grep "^INSERT INTO" | sort | uniq -c`.

FACT: games per league and their date ranges in the dump are MLB 8,534 (2024-03-20 to 2026-10-18), NBA 8,147 (2023-10-05 to 2026-10-17), NHL 4,676 (2023-10-01 to 2026-10-18), MLS 1,507 (2024-03-02 to 2026-10-18), NFL 1,085 (2023-09-08 to 2026-10-16) and EPL 825 (2024-08-16 to 2026-10-17), per the query "games per sport" in Sources. FACT: only 8 games in the whole table carry `odds_json`, per the same query. Assessment: odds were fetched 300 times but attached to almost no games, which matters for the spread record in section 4.8.

FACT: the `predictions` table is keyed `UNIQUE (game_id, model_version, prediction_source)` with columns for the pick, the probability, `reasoning_json`, `made_at`, `team_state_as_of`, `low_confidence`, and the resolution fields `resolved_at`, `actual_winner`, `was_correct` and `brier_score`, per the dump's `CREATE TABLE "predictions"` statement. FACT: `games` gained a `canonical_id` natural key (sport, date, home, away) on 2026-05-29 so that BallDontLie-sourced and ESPN-sourced rows for the same game could be deduplicated, per PR #87 in the commit log and the `idx_games_canonical_id` index in the dump.

FACT: the NBA box-score table stores per-team counts (field goals, threes, free throws, rebounds, assists, steals, blocks, turnovers, fouls, points, minutes) plus possessions computed at scrape time with the Oliver formula `FGA + 0.44*FTA - OREB + TOV`, and a `team_tov` column added later, per the `CREATE TABLE nba_game_box_stats` statement in the dump.

### 2.3 The predictors

FACT: winner prediction went through a ladder v0 to v3 and then v5, all in `src/analysis/predict.ts`. v0 picks home at the sport's empirical home-win rate (NBA 0.57, NFL 0.57, MLB 0.54, NHL 0.55, MLS 0.49, EPL 0.46); v1 flips when the visitor has ten more wins; v2 adds point-differential buckets; v3 adds a cold-streak penalty; v5 is a logistic curve, `prob_home = sigmoid(scale * (homeDiff - awayDiff + homeAdv + injuryAdj))`, clamped to [0.15, 0.85], falling back to the base rate when either team has fewer than five games, per `src/analysis/predict.ts` lines 60 to 290.

FACT: the final constants were home advantage NBA 2.25, NFL 2.5, MLB 0.5, NHL 0.3, MLS 0.4, EPL 0.4 (margin units); sigmoid scale NBA 0.10, NFL 0.10, MLB 0.30, NHL 0.45, MLS 0.80, EPL 0.90; and `INJURY_COMPENSATION = 0.4` (40 percent of a missing player's points per game counts), per `src/analysis/predict.ts` lines 171 to 256 and `Plans/mls-epl-sigmoid-scale.md` line 339. FACT: the scale derivation was `pi / (sqrt(3) * sigma_eff)` where sigma is the effective noise of predicting a single game from season averages, per the comment above `SIGMOID_SCALE` in `predict.ts`.

FACT: the spread model, `v4-spread`, predicted a home margin from the same differential and home advantage, attenuated the home advantage when the home team was on a cold streak (times 0.5) or the visitor on a hot streak (times 0.3), added an MLB pitcher term of plus or minus 0.3 runs per 1.0 of earned run average gap, compared the result to the median consensus line from the bookmakers, and labeled the pick Strong, Lean or Skip (shown as "Marginal") by the size of the edge, per `src/analysis/predict.ts` lines 302 to 384, `src/analysis/spread-runner.ts` lines 95 to 98 and `README.md` lines 303 to 315. FACT: the break-even cover rate quoted for a standard -110 line was 52.4 percent, per `README.md` line 315.

FACT: a soccer Poisson model (`src/analysis/poisson.ts`) computed independent Poisson goal rates from attack and defense strengths and a league home multiplier, giving a Skellam margin distribution with a draw probability; it was built and measured but never switched on as the production model, per `Plans/soccer-poisson.md` lines 17 to 40 and 132 to 143.

### 2.4 The ratchet loop

FACT: `src/orchestration/ratchet.ts` implemented hypothesize, modify, execute, evaluate, keep or revert, with improvement metrics set in `harness.yml`: Brier score for game outcomes, MAE for point spreads, root mean square error for player stats and Pearson r for trends, per `harness.yml` lines 46 to 55 and `README.md` lines 164 to 170. FACT: the loop ran once on NBA on 2026-04-07 and its artifact is `data/ratchet/nba-ratchet.json` (`runAt: 2026-04-07T12:17:38Z`). FACT: `harness.yml` set the ratchet to `semi_autonomous` with `dev_mode: true` ("Log gates but don't block during development") and 50 iterations per hypothesis.

Assessment: after that first run the "ratchet" became a discipline rather than a program. Every later model change was a pull request with a pre-declared ship rule and a council review; the automated loop in `ratchet.ts` was not the thing doing the iterating.

### 2.5 The council

FACT: five expert personas (data quality, statistical validity, prediction accuracy, domain expert, mathematics) plus a non-voting lead architect reviewed every plan, implementation and result with verdicts FAIL, WARN or CLEAR and a score out of ten, per `.harness/council/README.md` lines 7 to 30. FACT: the mathematics and prediction-accuracy experts abstained on reviews with no calculations, per `CLAUDE.md` in the repo ("Council discipline"). FACT: from 2026-04-30 (PR #65) the implementation and results reviews ran automatically on every pull request through Gemini, posting one synthesized comment, with a monthly cap of 60 runs, per `.github/workflows/council.yml` lines 1 to 12 and 103. FACT: the runner defaulted to `gemini-3.5-flash` after PR #90 retired `gemini-2.5-pro`, per `.harness/scripts/council.py` line 44. FACT: `harness.yml` line 11 still names `default_model: claude-sonnet-4-6`; Assessment: that value is a leftover from the day-one scaffold and the Gemini runner ignored it.

FACT: four process rules were codified out of incidents and are the clearest statement of what the council learned, per `.harness/council/README.md` lines 32 to 120:

- pm.5, dissenter-named falsification test: when a round-two reversal of a round-one FAIL rests on an empirical claim, the test the dissenting expert named in round one must be run first.
- pm.6, multi-row-write evidence standard: a reversal that pivots on data needs at least two data points per stratum, at least five in total, at least one point per stratum chosen by the dissenter, and the named falsification test.
- pm.7, data-prerequisite audit at plan review: a plan that names training, validation or test slices cannot be CLEAR until someone has queried the storage layer and confirmed the rows exist.
- pm.8, end-to-end smoke test for pipeline reviews: a feature or training pipeline cannot be CLEAR on unit tests alone; it needs a run on the real data path showing non-empty output, non-zero variance per feature group, both label classes present, and features that actually differ across hyperparameter values.

FACT: the repo-level rule that produced the most process text was "the user must never be the first reviewer", per `.harness/council/README.md` line 29 and `CLAUDE.md` "Council discipline". FACT: `.harness/memory/decisions.json` is an empty array.

### 2.6 The learned-model program

FACT: `ml/nba/` holds a Python pipeline: `features.py` (a rolling-window tensor of 42 to 49 features per game from the box-score table, with a time-machine filter so no feature uses data after the game date), `cv_runner.py` and `phase7_cv_runner.py` (forward-chaining inner CV), `train_lightgbm.py` and `train_mlp.py`, `calibrate.py` (Platt scaling in logit space), `evaluate_test_fold.py`, `evaluate_cold_start.py`, `bpm_prior.py` and `step7_preflight.py`, per the directory listing. FACT: a `test-fold-touch-counter.json` recorded every evaluation on the sealed test fold with the council co-signature, so the test fold could not be reused silently, per `ml/nba/test-fold-touch-counter.json`.

### 2.7 Deployment

FACT: the frontend was built with Vite and Observable Plot and served from Cloudflare Pages at `https://sportsdata.pages.dev`; the API ran as a Node process (`tsx src/viz/data-api.ts`) in a Docker image on Fly.io at `https://sportsdata-api.fly.dev`; both deployed on push to `main` through GitHub Actions, per `DEPLOY.md` "Topology" and `Dockerfile`. FACT: the Fly app is `sportsdata-api`, primary region `dfw`, VM size `shared-cpu-1x` with 512 MB, `auto_stop_machines = 'off'`, `min_machines_running = 1`, with the volume `sportsdata_vol` mounted at `/app/data` holding the SQLite file at `/app/data/sqlite/sportsdata.db`, per `fly.toml`. FACT: the design language was a white, Roboto, "Google Sheets energy" page in the style of Jon Bois's Chart Party and Dorktown videos, chosen by the council over a dark theme, per `design.md` and `docs/jon-bois-viz-style.md`.

## 3. What it found

The results below are the reason this note exists. They fall into three groups: what the hand-built models could and could not do, what five attempts at a learned NBA model showed, and what the live system actually recorded over six months of unattended running.

### 3.1 The ratchet ladder, v0 to v3 (NBA, April 2026)

FACT: on a 2,500-game NBA test set with a 2023 training cutoff, the ladder scored v0 (always home) Brier 0.4528 and accuracy 54.7 percent; v1 (flip on a ten-win gap) 0.3227 and 61.2 percent; v2 (plus point-differential buckets) 0.2486 and 61.4 percent; and v3 (plus a cold-streak penalty) 0.2508 and 61.8 percent, so v3 was reverted because its Brier was worse, per `data/ratchet/nba-ratchet.json`. FACT: the same artifact's Vegas comparison has `sampleSize: 3` and is labeled "PRELIMINARY ... Treat as instrumentation, not verdict", and no later Vegas comparison exists in the repository, per `data/ratchet/nba-ratchet.json` and the subagent's search of `SESSION_LOG.md`.

Assessment: the v0 Brier of 0.45 is not a fair baseline. The learnings file records that v0 originally predicted home with probability 1.0, which "inflated all improvement claims", and the code comment on `SPORT_HOME_WIN_RATE` in `src/analysis/predict.ts` says the council ordered the fix (FACT for both). The honest baseline Brier for a constant home rate is about 0.248 (see 3.3), so the real gain from v2 over "always home" on NBA is about 0.02 of Brier and about seven points of accuracy, not the "45 percent improvement" the session log celebrated.

### 3.2 v2 was overconfident and discrete

FACT: the calibration plot of Sprint 10 (2026-04-09) found that the NBA v2 model "only emits probability 60% or 75%"; the 60 percent bin (n = 1,495) hit 63.5 percent and the 75 percent bin (n = 1,005) hit 58.1 percent, "a 17-point overstatement", with ECE 0.0892 and verdict DISCRETE, per `SESSION_LOG.md` line 582. FACT: the historical backfill confirmed it across sports ("75% bucket actually won 50-62%"), per `learnings.md` line 235. This is what produced v5.

FACT: per-sport v2 backfill results in the final dump (12,863 rows, all resolved): NBA 5,150 games, 61.6 percent, Brier 0.249; NFL 617, 61.8 percent, 0.2389; MLB 3,546, 55.4 percent, 0.2673; NHL 2,822, 55.4 percent, 0.2667; MLS 494, 63.0 percent, 0.2465; EPL 234, 43.2 percent, 0.2536, per the query "v2 backfill accuracy by sport" in Sources. Assessment: a Brier above 0.25 means worse than saying 50-50 on every game, so v2 was actively harmful on MLB, NHL and EPL.

### 3.3 The per-sport baseline (2026-04-14): the one table that says what the hand-built models were worth

FACT: `data/baselines/baseline-2026-04-14.txt` replayed v5 (winner, no injury data) and v4-spread (margin, no pitcher or injury data) over 16,777 games after the 2023 training cutoff, with 1,000-resample bootstrap CIs and paired differences against two naive baselines, "predict zero margin" (nv0) and "predict the home advantage" (nvHA), and a constant home-rate Brier (nvBr). FACT: the file's own header says the constants were calibrated on the same corpus ("In-sample w.r.t. the parameter calibration").

| League | N | Margin MAE (v4-spread) | Naive MAE (nv0) | MAE minus nv0, 95 percent CI | Winner accuracy (v5) | Brier (v5) | Naive Brier | Verdict on Brier |
|---|---|---|---|---|---|---|---|---|
| NBA | 5,196 | 11.67 | 12.96 | -1.283 [-1.452, -1.116] | 63.1 percent | 0.2258 | 0.2477 | beats naive |
| NFL | 621 | 10.70 | 11.28 | -0.579 [-1.011, -0.146] | 63.1 percent | 0.2320 | 0.2487 | beats naive |
| MLB | 6,270 | 3.48 | 3.52 | -0.042 [-0.071, -0.014] | 56.0 percent | 0.2449 | 0.2485 | beats naive, barely |
| NHL | 2,832 | 2.16 | 2.23 | -0.071 [-0.095, -0.047] | 55.6 percent | 0.2444 | 0.2482 | beats naive, barely |
| MLS | 1,159 | 1.35 | 1.35 | -0.004 [-0.046, +0.035] | 63.1 percent | 0.2281 | 0.2417 | margin ties zero; Brier beats naive |
| EPL | 699 | 1.36 | 1.33 | +0.026 [-0.039, +0.093] | 66.0 percent | 0.2140 | 0.2474 | margin ties zero; Brier beats naive |

FACT: every number in the table is from `data/baselines/baseline-2026-04-14.txt`, "all" slice per league. FACT: on the held-out latest-20-percent slices, NFL, NHL and the EPL margin models could not be distinguished from the naive baselines at 95 percent, and the EPL margin model lost to predict-zero (+0.121 [+0.003, +0.242]), per the "test" blocks of the same file.

Assessment: the hand-built models carried real but modest signal in NBA and NFL (about one point of margin MAE and about two hundredths of Brier), almost none in MLB and NHL, and none for soccer margins. The winner accuracy numbers (55 to 66 percent) look better than the Brier numbers because home teams win more than half the time and the model mostly picks home; the Brier gap over a constant is the honest measure.

### 3.4 Reliability diagrams and the two one-number recalibrations

FACT: `data/reliability/reliability-2026-04-15.txt` binned every prediction and reported per-sport ECE with verdict words (HONEST within 0.02 of signed residual, SHY if under-confident, OVERCONFIDENT, BIASED_HIGH or BIASED_LOW for margins beyond 0.5 units). FACT: v5 winner ECE was NBA 0.0156, NFL 0.0515, MLB 0.0152, NHL 0.0162 (all HONEST), MLS 0.0429 (SHY) and EPL 0.0502 (SHY); the NBA margin model was BIASED_HIGH with a signed residual of -0.605 points uniform across all 20 populated bins, per the same file.

FACT: the NBA home advantage was then cut from 3.0 to 2.25 (debt #27, PR #34, 2026-04-20); the first attempt at 2.4 failed one of five pre-declared rules because the cold- and hot-streak attenuations fire more often than independence predicts (effective coefficient 0.809, not 0.926), and the corrected shift 0.605 / 0.809 = 0.75 passed all five rules, taking the NBA margin signed residual from -0.605 to +0.0012 at a v5 ECE cost of +0.0025, per `Plans/nba-home-adv-recalibration.md` lines 108 to 145. Assessment: the streak-coefficient lesson is small but real: in a league where quality persists, losing streaks cluster, so any constant gated behind a streak condition fires more than the naive rate.

FACT: the MLS and EPL sigmoid scales were raised from 0.60 to 0.80 and 0.90 by grid search, moving ECE from 0.0429 to 0.0380 (MLS) and 0.0502 to 0.0404 (EPL), both to HONEST, with Brier improving 0.2297 to 0.2283 and 0.2176 to 0.2144, per `Plans/mls-epl-sigmoid-scale.md` lines 339 to 358. FACT: the selection rule was minimum absolute signed residual rather than minimum ECE, because EPL ECE was non-monotonic in the scale (0.0502 at 0.60, peak 0.0654 at 0.80, 0.0404 at 0.90, 0.0323 at 1.00), per the same file lines 203 to 218.

### 3.5 Soccer: the Poisson null and a proof that saved a sprint

FACT: the Poisson model's pre-declared primary gate was "Poisson MAE minus predict-zero MAE, CI entirely below zero, on both MLS and EPL"; it produced MLS -0.007 [-0.049, +0.032] and EPL +0.007 [-0.053, +0.067], both ties, so it did not ship, even though on EPL it beat v4-spread by -0.019 [-0.036, -0.002], per `Plans/soccer-poisson.md` lines 136 to 143. FACT: the minimum detectable effect at N = 699 was about 0.13 MAE, an 8 to 10 percent reduction, per the same file line 97. Assessment: the honest reading recorded in the plan is "N is too small", not "Poisson does not work"; the fix the council endorsed was a pre-2024 soccer scrape (debt #26) that was never done.

FACT: when the next step was proposed as "add the Dixon-Coles low-score correction", the mathematics expert proved that the tau correction cannot change the expected margin (the four corrected cells shift the expectation by exactly offsetting amounts and the normalizer is exactly 1), so it could never move the MAE gate, per `Plans/soccer-poisson.md` lines 145 to 167. Assessment: this is the cleanest example in the project of writing down which metric a change can move before building it.

### 3.6 NBA learned models: five null results, Phases 1 to 7

FACT: the whole program is in `Plans/nba-learned-model.md` (3,219 lines, addenda v1 to v23) with companion plans `Plans/nba-cold-start-prior.md`, `Plans/nba-phase5-bug-fixes.md` and `Plans/nba-phase6-season-aggregate.md`, and its artifacts in `ml/nba/results/` and `ml/nba/test-fold-touch-counter.json`. The incumbent throughout was v5, the sigmoid on season point differential.

**Phase 1 (2026-04-24), rolling-window point differential, stopped at the premise check.** FACT: the hypothesis was that a last-N-games point differential carries more signal than the season differential; the pre-declared premise gate was a Pearson r gain of at least 0.02; the best rolling window (N = 20) gave r = 0.4288 against the season figure 0.4157, a gain of 0.0131, so the premise failed and no model was built, per `Plans/nba-learned-model.md` lines 424 to 438 and 513 to 521. FACT: the v5 Brier on the 2024-25 validation fold (N = 1,321) was 0.2161, per line 424.

**Phase 2 (2026-04-25 to 04-26), box-score data, shipped.** FACT: 7,604 team-rows for 3,802 games across the 2023-24, 2024-25 and 2025-26 seasons were backfilled from ESPN at 100 percent coverage, and a 50-game cross-source audit against basketball-reference ended at 0 raw-count, 0 derived-rate and 0 missing failures, per `Plans/nba-learned-model.md` lines 801 and 1007 to 1017 and `docs/espn-bbref-audit-v19.md`.

**Phase 3 (2026-04-27 to 04-28), LightGBM and MLP on 42 EWMA features, null.** FACT: training pool 2,640 games (2023-24 and 2024-25 including playoffs), EWMA half-life 21 games, a 20-seed LightGBM ensemble (num_leaves 31, min_child_samples 200, reg_alpha 1.0) and a 42-128-64-1 MLP; the pre-declared ship rule was a Brier gain of at least 0.010 with a block-bootstrap CI excluding zero, behind a halt gate requiring AUC at least equal to v5's, per `Plans/nba-learned-model.md` lines 2025, 2162 to 2163, 2279 and 2380 to 2384. FACT: on the sealed 2025-26 regular-season test fold (N = 1,162), v5 scored AUC 0.7283 and Brier 0.209259; LightGBM scored AUC 0.6954 and Brier 0.222185 (worse by 0.012926); the MLP scored AUC 0.7026 and Brier 0.217956 (worse by 0.008697), per `ml/nba/test-fold-touch-counter.json` touches 1 and 2 and `Plans/nba-learned-model.md` lines 2404 to 2487. FACT: the validation-fold signal before the test had been +0.0065 at a standard error of about 0.005, about 1.3 sigma, per `learnings.md` line 596.

FACT: the post-mortem named four causes: EWMA cold start (about 186 early-season games, 16 percent of the test fold, had near-zero feature signal); the validation fold contained 84 playoff games while the test fold had none; a scaling bug stored turnover percentage as 5 to 25 instead of 0.05 to 0.25, so the logit normalization saturated and four of 42 features were constant; and season-aggregate point differential is near-sufficient on its own, per `Plans/nba-learned-model.md` lines 2429 to 2443 and `learnings.md` line 595.

**Phase 4 (2026-04-28), BPM cold-start prior, null.** FACT: a player-aggregate prior from basketball-reference BPM weighted by minutes, blended with the season differential as `(K * prior + g * actual) / (K + g)` with K = 10, was added as features 43 and 44; on games 1 to 20 of the 2025-26 test season (n = 309) the model scored Brier 0.210309 against v5's 0.205528, a difference of +0.004781 with CI [-0.010219, +0.019839], so ship rule 1 failed; AUC was 0.7241 against 0.7283, per `Plans/nba-cold-start-prior.md` lines 516 to 541. FACT: the rookie-prior calibration on 2010 to 2021 draft classes with a 500-minute filter gave median first-season BPM of -2.25 (picks 1 to 5), -2.05 (6 to 14), -2.40 (15 to 30), -2.60 (second round) and -0.50 (undrafted, with the file noting survivor bias), and the K grid was flat across [5, 15], per the same file lines 430 to 436 and 464 to 478. FACT: traded players, 13 to 14 percent of active players per season, were excluded from the prior because basketball-reference reports them as multi-team rows, per line 512.

**Phase 5 (2026-04-29), bug fixes, null.** FACT: with the turnover scaling fixed and the validation fold restricted to regular-season games, LightGBM scored AUC 0.7221 and Brier 0.211973 (worse than v5 by 0.002714), per `Plans/nba-phase5-bug-fixes.md` lines 937 to 947.

**Phase 6 (2026-04-29), season net rating added as explicit features, null.** FACT: adding `season_net_rating` for both teams (v5's own signal, pace-adjusted) moved AUC to 0.7237 and Brier to 0.211391 (worse by 0.002132); the plan had pre-declared that an AUC gain under 0.003 meant "the features are not providing the expected signal", and the gain was +0.0016, per `Plans/nba-phase6-season-aggregate.md` lines 760 to 772. FACT: the AUC progression across phases was 0.6954 (Phase 3), 0.7241 (Phase 4), 0.7221 (Phase 5), 0.7237 (Phase 6), against v5's 0.7283, per the same file lines 774 to 781. FACT: the domain expert's reading was that "v5 applies a fixed coefficient (0.10) to a hand-crafted quality score with explicit home advantage (2.25). LightGBM has to discover those coefficients from data, and consistently falls short", per line 789.

**Phase 7 (2026-05-01 to 05-24), hybrid season-aggregate plus EWMA-delta, null.** FACT: the design dropped playoffs, trained on the 2021-22 and 2022-23 regular seasons (2,447 effective games after exclusions), validated on 2023-24 (N = 1,237), sealed 2024-25 (N = 1,237) as the test fold, used 49 features (20 season-aggregate, 20 EWMA-delta at half-life 14, 9 game-level), and a single-seed LightGBM (num_leaves 63, min_child_samples 100, reg_alpha 0.1) with Platt calibration; the ship rule was a Brier gain of at least 0.005 with the block-bootstrap CI excluding zero, per `ml/nba/results/phase7-val-20260524T115328Z-3c730bbc.json` and `Plans/nba-learned-model.md` lines 2574 to 2601 and 2987 to 2999. FACT: the result on the validation fold was Phase 7 Brier 0.222617 against v5 0.214916, a difference of -0.0077 with block-bootstrap CI [-0.0144, -0.0008] (651 blocks of home team by ISO week, 10,000 resamples) and ECE 0.0304 against 0.0249; the ship gate was FAIL and the test fold was never touched, per the same artifact. FACT: the inner-CV half-life selection gave mean Brier 0.2346 (h = 7), 0.2339 (h = 14) and 0.2354 (h = 21), per `ml/nba/results/phase7-cv-20260524T094619Z-135677f1.json`.

FACT: two pipeline bugs were found during Phase 7 and are the origin of rules pm.7 and pm.8: the training seasons named in a council-CLEAR plan had zero box-score rows because the Phase 2 backfill was scoped to 2023 onward (addendum v19), and a time-machine filter on `updated_at` excluded every backfilled row so that an early run produced an identical Brier of 0.2459930364898013 for all three half-lives (addendum v21), per `Plans/nba-learned-model.md` lines 2672 and 2848 to 2871. FACT: the 2021 and 2022 backfill reached 99.7 percent aggregate coverage (12,498 of 12,536 team-game cells across all seasons; 4,894 new rows for 2021 and 2022), with the 19 missing games clustered in the Omicron window of 2021-12-14 to 2022-02-03 where ESPN's responses lacked a field-goal aggregate or the game was rescheduled, per `docs/phase7-2021-2022-backfill-coverage.md`.

**The cross-phase lesson, quoted.** FACT, from `Plans/nba-learned-model.md` line 3134: "Architectural lesson (corroborated by 5 independent attempts): for NBA win-probability prediction at n=4k games of training data, the v5 simple-sigmoid baseline on season-aggregate point differential is empirically near-sufficient. Adding feature complexity (EWMA windowing, recency-delta, player-aggregate priors, season-aggregate companions) has not produced a robust Brier improvement of >=0.005 with bootstrap-CI excluding zero." FACT, line 3136: "the model with 89 features performs worse than v5 with 3 statistics (games_played, points_for, points_against)." FACT: the plan's own list of what was not falsified is injury, schedule and travel features, signals beyond box scores, and a different model class, per lines 3144 to 3148; the five Phase 8 candidates (injury features, schedule and travel, BPM with the trade fix, declaring v5 permanent and moving to other sports, a Bayesian or Gaussian-process model) were filed as debt #37 and never picked up, per lines 3165 to 3173.

Assessment: the lesson generalizes beyond this project. With a few thousand games, two seasons of history and features that are all transformations of the same box scores, a gradient-boosted model has to rediscover a linear relationship that a human wrote down in one line, and it pays a variance cost for the privilege. The experiments were well run (sealed test folds, paired block-bootstrap CIs, pre-declared gates, a touch counter), so the negative result is trustworthy. Speculation: an injury or rest signal timed per game is the only direction on the list that adds information the box score does not already contain, and it is the one the project never had the data for (see 3.9).

### 3.7 Data-quality findings

FACT: ESPN exposes three turnover fields (`turnovers`, player-summed; `teamTurnovers`; and `totalTurnovers`, their sum) and basketball-reference's team TOV matches `totalTurnovers` for about 99.8 percent of games; the exception is NBA Cup knockout games (about 14 games, 0.18 percent), where bbref uses the player sum, per `Plans/nba-learned-model.md` lines 1051 to 1059, 1273 to 1280 and 1418. FACT: a plan that switched the stored convention to the player sum (addendum v10) passed council at 9.2 and 9.1 out of 10 on the evidence of one Cup-final game, was backfilled into 7,604 rows, failed the cross-source audit with 57 raw and 139 rate failures, and was rolled back by rescrape, after which the per-season average TOV matched the pre-change value to 15 significant figures, per lines 1237 to 1267 and 1331 to 1344. Assessment: the generic lesson, recorded in `learnings.md` line 522 as "the discipline lives with the proponent, not the dissenter", is that a single confirming example chosen by the person proposing the change is not evidence.

FACT: the possessions formula differs between sources (Oliver's 0.44 times free-throw attempts minus offensive rebounds, versus bbref's 0.4 and an offensive-rebound-rate term), which put the project's possession counts about 2.5 percent (about 1.9 per game) high against bbref and failed 200 of 200 offensive-rating and pace comparisons until the audit's own formula was decoupled from the stored value, per `Plans/nba-learned-model.md` lines 824 to 830 and 926 to 931. FACT: ESPN's minutes-played drifts to 241 to 243 instead of 240 in about 6 percent of games, per lines 938 to 954.

FACT: v5 performs badly on NBA Cup knockout games: Brier 0.2861 on 14 such games against 0.2046 on 386 regular-season games in the same months, a difference of 0.0816 with bootstrap CI [0.0105, 0.1671], per `docs/cup-knockout-disposition-evidence.md`. Assessment: with n = 14 this is a flag, not a finding; it says the model has no idea about one-off neutral-site tournament games, which is plausible.

FACT: ESPN's injury endpoint dropped the `athlete.id` field at some point before 2026-05-28, collapsing every injury row onto one primary key until the scraper was changed to parse ids from URLs, per PR #85 in the commit log. FACT: the `player_injuries` table holds only the latest snapshot (primary key player and sport, 1,343 rows all fetched 2026-10-14), so no per-game injury history exists in the data, per the dump schema and the query "player stats, injuries" in Sources.

### 3.8 The live track record as of 2026-10-14

This is what the deployed system actually recorded, computed by this session from the final backup. The numbers are FACT from the queries listed in Sources; the readings are Assessment.

FACT: the `predictions` table holds 12,863 v2 backfill rows (made 2024-09 to 2026-04-11), 197 live v2 rows (2026-04-07 to 04-13), 765 live v5 rows and 526 live v4-spread rows (both 2026-04-13 to 2026-10-09), per the query "predictions by model and source". FACT: live predictions were made in April (900), May (425) and October 2026 (163) and in no other month, per the query "predictions made per month"; Assessment: the predict step did not run from June through September, which section 4 explains.

FACT: live v5, resolved and not low-confidence, 644 predictions: 336 correct (54.9 percent), Brier 0.2444. By league, with the always-pick-home rate and the constant-rate Brier over the same games for comparison: MLB 404 games, 51.1 percent against 53.2 percent home, Brier 0.2528 against 0.2490; NHL 78, 57.7 against 52.6, 0.2391 against 0.2496; NBA 59, 67.8 against 61.0, 0.2185 against 0.2416; MLS 44, 65.9 against 61.4, 0.2276 against 0.2641; EPL 28, 64.3 against 57.1, 0.2112 against 0.2695, per the query "live v5 by sport vs always-pick-home". Assessment: in live use the model was worse than a constant on MLB, which was two thirds of its volume, and better than a constant on the other four leagues at sample sizes too small to say much. The live Brier of 0.2444 across all sports is about the naive figure, and the live 54.9 percent accuracy is below the 56 to 63 percent of the in-sample baseline. The in-sample caveat the project wrote on every artifact turned out to matter.

FACT: live v5 calibration in 5-point bins: the 0.50 bin (n = 257) predicted 0.525 and hit 0.547; the 0.55 bin (n = 177) predicted 0.571 and hit 0.401; the 0.60 bin (n = 102) predicted 0.622 and hit 0.660; higher bins have n of 61 or fewer, per the query "live v5 calibration". Assessment: a 17-point miss on the second-largest bin is the kind of thing the reliability diagrams were built to catch, and nobody was looking.

### 3.9 What it never measured, and the one record that was wrong

FACT: the stored against-the-spread record is not a cover record. Of 475 resolved live v4-spread picks, 419 were matched to games whose `game_results.spread_result` was NULL, and the resolver scores a NULL or push as a miss (`src/analysis/resolve-predictions.ts` line 153, "if (!c.spread_result || c.spread_result === 'push')"), so `was_correct` sums to 39 of 474, a 0 percent "cover rate" in EPL, MLS and NHL and 2 percent in NBA, per the queries "v4-spread live resolved: spread_result on the matched game" and "live overall by model". Assessment: `spread_result` is only written when a game has `odds_json` at resolution, and only 8 games ever did, so the frontend's "Track record gated at N>=30 resolved picks" (`README.md` line 315) was gated on a number that could only be near zero. This is a bug, not a finding about the model.

FACT: recomputing covers from the line stored in each pick's `reasoning_json` and the final score (474 picks, 4 pushes): Lean 115 covers of 182 decided (63.2 percent), Strong 54 of 86 (62.8 percent), Skip 97 of 177 (54.8 percent); by league, MLB 175 of 285 (61.4 percent), NHL 37 of 53 (69.8 percent), NBA 26 of 50 (52.0 percent), MLS 20 of 34 (58.8 percent), EPL 8 of 23 (34.8 percent), per the query "recomputed ATS". FACT: 245 of 285 MLB picks and 49 of 53 NHL picks took the underdog side, and the line was 1.5 in 276 of the MLB picks and 51 of the NHL picks, per the query "which side do the picks take". Assessment: the MLB and NHL "edges" are the run-line and puck-line underdog base rates. A +1.5 underdog covers in roughly six MLB games in ten and seven NHL games in ten because favorites rarely win by two or more, and bookmakers price those sides far below the -110 the project's 52.4 percent break-even assumed, so these rates show no profitable edge. The NBA point-spread sample (50 picks, 52 percent) is a coin flip at a sample size that could not detect anything. Speculation: had the project ingested historical closing lines (debt #20, never done), it would have found the same thing on ten times the data.

FACT: the shadow A/B for the injury signal, built in PR #38 to write a `v5-naive` row beside every injury-adjusted `v5` row, never produced a single pair: the final dump contains no `v5-naive` or `v4-spread-naive` rows at all, per the query "predictions by model and source". FACT: the plan's expected power ("N=30 in 2 weeks" for NBA and MLB) is in `Plans/shadow-prediction-logging.md` lines 531 to 536, and debt #32 (the shadow-analysis report) was still open at the end, per `BACKLOG.md` line 76. Assessment: the injury adjustment, the one feature the domain expert kept naming as the likely source of real signal, shipped on 2026-04-13 and was never measured out of sample.

FACT: the "Interesting Things" detector surfaced the findings the frontend led with: Detroit's 28-game losing streak, Oklahoma City's 62-point win over Portland, and teams nearest .500 with high alternation, per `README.md` lines 174 to 184. Assessment: these are facts about basketball, not findings of the project; they are listed here because they are what a visitor to the site saw.

## 4. What it cost and why it died

The project cost almost nothing in money and died because the only thing still running was a cron job that nobody read, posting to an API that answered with a timeout.

### 4.1 Cost

FACT: the paid component was the Fly.io machine `sportsdata-api`, `shared-cpu-1x` with 512 MB, always on (`min_machines_running = 1`, `auto_stop_machines = 'off'`), plus the volume `sportsdata_vol`, per `fly.toml`. Unverified (2026-10-15): Fly's list price for that machine class was a few dollars per month plus volume storage per gigabyte; the real charge is only in the dashboard at https://fly.io/dashboard and the published rates are at https://fly.io/docs/about/pricing/, neither of which this session could read. FACT: the memory was raised from 256 MB to 512 MB and the machine kept always-on in Sprint 7 to fix slow cold starts, per `SESSION_LOG.md` lines 443 to 454 and the commit "Fix slow cold starts: bundle tsx, bump memory, always-on machine" of 2026-04-07.

FACT: Cloudflare Pages, GitHub Actions on a public repository, the ESPN API, BallDontLie's free tier and The Odds API's 500 requests per month were all free, per `SESSION_LOG.md` lines 302 to 310 and `.env.example`. FACT: the Gemini council was capped at 60 runs per month and ran only on pull requests, so it spent nothing after 2026-07-10, per `.github/workflows/council.yml` line 103 and the commit log. FACT: no dollar figure for any service appears anywhere in the repository, per the subagent's full read of `SESSION_LOG.md` and `learnings.md`.

FACT: the time cost the project recorded for itself: about six hours of plan, implementation and review for about 32 minutes of production runtime on Phase 2, and about 30 minutes of "structured thought" per council addendum, per `learnings.md` lines 497 and 512. Assessment: that ratio is the price of the process that made the null results trustworthy, and it is also why the project stopped: every further hypothesis cost a day of review before a line of model code could be written.

### 4.2 Why it died

FACT: the Predict Cron workflow's 400 most recent runs, by month, were April 2026 36 successes and 29 failures; May 11 and 52; June 0 and 60; July 0 and 62; August 0 and 61; September 0 and 60; October (to the 15th) 3 and 26, per `gh run list -R Anguijm/sportsdata --workflow=predict-cron.yml --limit 400` on 2026-10-15. FACT: the last success before the long gap was the run preceding 2026-05-29 23:03 UTC, and the three October successes were on 2026-10-08 01:26, 2026-10-08 11:45 and 2026-10-09 01:33 UTC, per the same listing. FACT: the newest run (id 38658535571, 2026-10-15 00:10 UTC) failed in the "Trigger scrape on Fly.io" step with four consecutive `curl: (22) The requested URL returned error: 502` lines about 78 seconds apart and `scrape_http=502`, per `gh run view 38658535571 --log-failed`.

FACT: the database kept changing through the dead months: `games.updated_at` falls in June 2026 for 418 rows, July 416, August 535, September 648 and October 337, per the query "games updated_at per month". FACT: on the day of the audit the health endpoint reported a scrape completed at 2026-10-15 00:14:59 UTC, four minutes into a run that had already failed, per the audit card `10_staging/project-audit/cards/sportsdata.md`. Assessment: the scrape handler kept running to completion on the machine while the HTTP request that started it was cut off by the Fly edge with a 502, so every run failed at step one and the predict step, which only runs after a successful scrape, never fired. That is why there are no predictions from June through September and why the frontend showed stale picks. The three October successes line up with the NBA and NHL seasons starting (fewer MLB games per day); Speculation: a six-league scrape with a four-day backfill window grew past the edge timeout as the database and the daily game count grew, and the cron's own retry logic (three retries, written for five-second deploy blips) made a slow endpoint look like a dead one. Unverified (2026-10-15): the exact Fly proxy timeout that produces the 502; Fly's documentation is at https://fly.io/docs/ and was not read for this note.

FACT: the two other scheduled workflows kept working: "Backup SQLite DB" succeeded daily through 2026-10-14 and pruned to a seven-day window (releases `backup-2026-10-06` to `backup-2026-10-14` existed on 2026-10-15), and "Harness Drift Check" succeeded weekly from 2026-08-10 to 2026-10-12, per `gh release list` and `gh run list --workflow=drift-check.yml`. FACT: the project's own runbook said "Any run older than 36 hours or marked `failure` needs investigation", per `DEPLOY.md` "Check cron health". Assessment: the alerting worked exactly as designed (every failed run printed "Predict cron failed!") and nobody was subscribed to it. The operator's last touch was the harness-cli adoption on 2026-07-10; the cron had already been red for six weeks by then.

FACT: the operator's ruling on 2026-10-15 was "Sports Data Predictor? We can kill it, I don't care. Document the research that we did and the findings, and then kill the whole repo after that."

## 5. What is worth keeping

Three things earned their place: a method for running model experiments that cannot fool their author, a schema and data set that another project could load tomorrow, and one negative result that saves anyone with the same idea two months.

### 5.1 The method

Assessment, drawn from `learnings.md`, `.harness/council/README.md` and the plans: the practices below are the ones that made the null results worth having, and every one of them was written down because the opposite had already gone wrong once in this repository.

- Pre-declare the ship rule, the metric and the threshold in a plan file before writing model code, and never move it afterward. The soccer Poisson plan (`Plans/soccer-poisson.md`) is the template; `learnings.md` line 333 records that this "made the null result usable rather than demoralizing".
- Report paired differences with bootstrap CIs, blocked by team and week, not independent intervals on the model and the baseline. The paired standard deviation was about 0.114 against a marginal 0.20 (`.harness/learnings.md` line 40).
- Seal the test fold, count every touch, and require a council signature per touch (`ml/nba/test-fold-touch-counter.json`). Evaluate the cheap halt gate (AUC at least the incumbent's) before the expensive ship rule.
- Require a validation-fold point estimate at least two standard errors above the ship floor before touching the test fold (`learnings.md` line 596); Phase 3 touched at 1.3 sigma and lost.
- Match the validation and test folds in regime (regular season only), and calibrate the planning sigma from held-out data; the planning value 0.095 against the realized 0.151 made one gate three times stricter than intended (`learnings.md` line 570).
- Before any production backfill, snapshot the database, and treat a single proponent-chosen example as no evidence (pm.5 and pm.6).
- Verify at plan time that the named data slices exist (pm.7), and smoke-test a pipeline on the real data path before review (pm.8).
- Keep an in-sample caveat on every artifact whose constants were fit on the corpus it is scored on. Section 3.8 is what happens when that caveat is right.
- Write down, before building anything, which metric a change can move. The Dixon-Coles proof (`Plans/soccer-poisson.md` lines 145 to 167) killed a sprint of work in a paragraph.
- A null result is a result; reserve "shipped" for things that passed the pre-declared rule.

### 5.2 The schema and the data

FACT: the full schema is in the backup dump (`/home/johnanguiano/backups/sportsdata/release-backup-2026-10-14/sportsdata.sql.gz`, 4,192,143 bytes, gzip verified, loads cleanly). Assessment: the parts worth reusing are the `predictions` table design (one row per game, model version and source, with the variant encoded in `model_version` so shadow models need no migration; `Plans/shadow-prediction-logging.md` lines 395 to 408), the `provenance_json` column on every scraped row, the `canonical_id` natural key across scraper namespaces, and the `nba_game_box_stats` table with its pinned possessions formula and `first_scraped_at` versus `updated_at` audit stamps.

FACT: the data that exists nowhere else: 24,774 games with 24,563 results across six leagues from 2023 to 2026-10-18 (the Fly dump); 7,604 NBA box-score team-rows for 2023-24 to 2025-26 (the Fly dump); and 4,894 additional box-score rows for 2021-22 and 2022-23 that were backfilled on the laptop on 2026-05-24 and never reached Fly, which this session copied from the laptop clone's untracked `data/sqlite/sportsdata.db` to `/home/johnanguiano/backups/sportsdata/laptop-local-data/sportsdata-laptop-2026-05-24-with-2021-2022-box-stats.db` (30,101,504 bytes, 12,498 box-score rows, integrity check ok). FACT: the 140 trained model files under `ml/nba/results/` are gitignored and were copied to `/home/johnanguiano/backups/sportsdata/laptop-local-data/ml-nba-results/` (11 MB), along with the four pre-backfill snapshots and the baseline, reliability and ratchet artifacts.

FACT: the code is preserved whole in the bare mirror `/home/johnanguiano/backups/sportsdata/sportsdata.git` (5.1 MB, head `8e42758`, all 37 branches that existed on GitHub on 2026-10-15 and the 9 backup tags, per `git for-each-ref refs/heads | wc -l` against `git ls-remote --heads`) and in the laptop clone at `/home/johnanguiano/projects/sportsdata`, which also carries 20 remote-tracking refs for branches that had already been deleted on GitHub, per `git remote prune origin --dry-run`.

### 5.3 The finding

Assessment: if someone asks whether a machine-learned model beats a sigmoid on point differential for NBA win probability with a few seasons of box scores, the answer this project paid for is no, five times, with CIs. The documents that carry it are `Plans/nba-learned-model.md` addendum v23 (lines 3124 to 3173), `Plans/nba-cold-start-prior.md`, `Plans/nba-phase5-bug-fixes.md`, `Plans/nba-phase6-season-aggregate.md`, `ml/nba/test-fold-touch-counter.json` and `ml/nba/results/phase7-val-20260524T115328Z-3c730bbc.json`, all in the mirror. The reusable constants are in section 2.3, the rookie BPM priors in section 3.6, and the streak-attenuation coefficient (0.809) in section 3.4.

Speculation: the two directions that remained open and plausible were per-game injury and rest data (never collected; the only snapshot table overwrote itself) and historical closing lines (never ingested). Either would need data the free tiers did not provide.

## 6. Sources

Files read for this note, all in the laptop clone `/home/johnanguiano/projects/sportsdata` at commit `8e42758` unless stated. Line numbers for `Plans/nba-learned-model.md`, `SESSION_LOG.md` and `learnings.md` come from two Explore subagents that read those files in full and reported with line references; everything else was read directly.

Repository documents: `README.md`; `CLAUDE.md`; `DEPLOY.md`; `design.md`; `BACKLOG.md`; `SESSION_HANDOFF.md`; `SESSION_LOG.md` (102 KB); `learnings.md` (112 KB); `harness.yml`; `fly.toml`; `Dockerfile`; `.env.example`; `MEMORY/WORK/20260404-100943_sportsdata-platform-research-council/PRD.md`.

Harness and council: `.harness/README.md`; `.harness/council/README.md`; `.harness/learnings.md`; `.harness/memory/context.md`; `.harness/memory/decisions.json`; `.harness/model-upgrade-audit.md`; `.harness/active_plan.md`; `.harness/scripts/council.py` (lines 15, 44).

Plans: `Plans/nba-learned-model.md` (3,219 lines); `Plans/nba-cold-start-prior.md`; `Plans/nba-phase5-bug-fixes.md`; `Plans/nba-phase6-season-aggregate.md`; `Plans/nba-phase2-backfill.md` (first 60 lines); `Plans/soccer-poisson.md`; `Plans/nba-home-adv-recalibration.md`; `Plans/mls-epl-sigmoid-scale.md`; `Plans/shadow-prediction-logging.md`; `Plans/reliability-diagrams.md`; `Plans/sprint3-plan.md` and `Plans/goofy-wibbling-fern.md` (headers).

Docs and data artifacts: `docs/espn-bbref-audit.md`; `docs/espn-bbref-audit-v19.md` (first 100 lines); `docs/bbref-convention-report.md`; `docs/phase-3-game-type-handling.md`; `docs/cup-knockout-disposition-evidence.md` (first 80 lines); `docs/phase7-2021-2022-backfill-coverage.md`; `docs/jon-bois-viz-style.md`; `data/baselines/baseline-2026-04-14.txt`; `data/reliability/reliability-2026-04-15.txt`; `data/ratchet/nba-ratchet.json`; `ml/nba/results/phase7-val-20260524T115328Z-3c730bbc.json`; `ml/nba/results/phase7-cv-20260524T094619Z-135677f1.json`; `ml/nba/test-fold-touch-counter.json`; `ml/nba/test-fold-touch-audit.log`.

Source code: `src/analysis/predict.ts` (lines 1 to 200 and grep of constants); `src/analysis/spread-runner.ts` (grep); `src/analysis/resolve-predictions.ts` (grep, lines 18, 145 to 171, 650 to 677); `skills/10-scrape.md` and `skills/20-analyze.md` (headers); directory listings of `src/`, `ml/nba/`, `scripts/`.

Workflows: `.github/workflows/predict-cron.yml`; `backup-db.yml`; `drift-check.yml`; `council.yml` (first 60 lines and grep).

Commands run on 2026-10-15: `git log origin/main --format='%ad %s' --date=short` (226 lines) and `--date=iso` for the first and last five commits; `git branch -r | wc -l` (57); `gh pr list -R Anguijm/sportsdata --state all --limit 1` (91); `gh release list -R Anguijm/sportsdata`; `gh run list -R Anguijm/sportsdata --workflow=predict-cron.yml --limit 400 --json conclusion,createdAt`; the same for `drift-check.yml` and `backup-db.yml`; `gh run view 38658535571 -R Anguijm/sportsdata --log-failed`; `gh auth status`.

The 2026-10-14 dump: `/home/johnanguiano/backups/sportsdata/release-backup-2026-10-14/sportsdata.sql.gz`, loaded with `zcat ... | sqlite3 sd.db`. The queries named in the text, in order, were:

- "games per sport": `SELECT sport, COUNT(*), MIN(date), MAX(date), SUM(odds_json IS NOT NULL) FROM games GROUP BY sport;`
- "predictions by model and source": `SELECT model_version, prediction_source, COUNT(*), SUM(resolved_at IS NOT NULL), MIN(made_at), MAX(made_at) FROM predictions GROUP BY 1,2;`
- "v2 backfill accuracy by sport": `SELECT sport, COUNT(*), AVG(was_correct), AVG(brier_score) FROM predictions WHERE model_version='v2' AND prediction_source='backfill' AND resolved_at IS NOT NULL GROUP BY 1;`
- "live overall by model": `SELECT model_version, COUNT(*), SUM(was_correct), AVG(was_correct), AVG(brier_score) FROM predictions WHERE prediction_source='live' AND resolved_at IS NOT NULL AND low_confidence=0 GROUP BY 1;`
- "live v5 by sport vs always-pick-home": `SELECT p.sport, COUNT(*), AVG(p.was_correct), AVG(r.home_win), AVG(p.brier_score), AVG((r.home_win - s.rate)*(r.home_win - s.rate)) FROM predictions p JOIN game_results r ON r.game_id=p.game_id JOIN (SELECT sport, AVG(home_win) rate FROM game_results GROUP BY sport) s ON s.sport=p.sport WHERE p.model_version='v5' AND p.prediction_source='live' AND p.resolved_at IS NOT NULL AND p.low_confidence=0 AND r.is_draw=0 GROUP BY 1;`
- "live v5 calibration": `SELECT ROUND(CAST(predicted_prob*20 AS INT)/20.0,2), COUNT(*), AVG(predicted_prob), AVG(was_correct) FROM predictions WHERE model_version='v5' AND prediction_source='live' AND resolved_at IS NOT NULL AND low_confidence=0 GROUP BY 1;`
- "predictions made per month": `SELECT substr(made_at,1,7), prediction_source, COUNT(*) FROM predictions GROUP BY 1,2;`
- "games updated_at per month": `SELECT substr(updated_at,1,7), COUNT(*) FROM games GROUP BY 1;`
- "v4-spread live resolved: spread_result on the matched game": `SELECT r.spread_result, COUNT(*), SUM(p.was_correct) FROM predictions p JOIN game_results r ON r.game_id=p.game_id WHERE p.model_version='v4-spread' AND p.prediction_source='live' AND p.resolved_at IS NOT NULL GROUP BY 1;`
- "recomputed ATS": from each live, resolved, non-low-confidence, non-draw v4-spread pick, take `json_extract(reasoning_json,'$.spread.pick_side')`, `json_extract(reasoning_json,'$.spread.spread_line')` (the home line) and `json_extract(reasoning_json,'$.spread.confidence_tier')`, with `hm = home_score - away_score`; a home pick covers when `hm + line > 0`, an away pick when `hm + line < 0`, and `hm + line = 0` is a push; grouped by sport and tier.
- "which side do the picks take": same base, counting picks where `(side='home' AND line>0) OR (side='away' AND line<0)` and picks where `ABS(line)=1.5`.
- "player stats, injuries": `SELECT sport, COUNT(*) FROM player_stats GROUP BY 1;` and `SELECT sport, COUNT(*), MIN(fetched_at), MAX(fetched_at) FROM player_injuries GROUP BY 1;`

Laptop-only data: `/home/johnanguiano/projects/sportsdata/data/sqlite/sportsdata.db` (untracked, 2026-05-24), queried with `SELECT season, COUNT(*) FROM nba_game_box_stats GROUP BY 1;`.

The audit card that preceded this note: `/home/johnanguiano/brain/10_staging/project-audit/cards/sportsdata.md` (2026-10-15).
