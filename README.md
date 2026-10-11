# Football Model Lab — Prototype 09

An editable Streamlit NFL/College Football dashboard with separate historical
training workflows and an explicit college-football selection profile.
Python 3.11 or 3.12 is recommended.

## Update your deployed app

1. Extract `football_dashboard_prototype09.zip`.
2. In GitHub, open the folder containing the deployed `app.py`.
3. Upload **all contents** of the extracted `football_dashboard` folder there,
   replacing existing files. Include the new Python modules and updated
   `requirements.txt`. Do not upload just the ZIP or create an extra nested folder.
4. Commit to the branch your Streamlit app deploys from. Refresh the app after
   dependencies install; use Manage app → Reboot app if needed.
5. Check the version display reads **Prototype 09**.

The download is an updated deployment package; it does not change your live
GitHub repository or Streamlit deployment until you upload it.

## New in this release

- **Historical training fix:** a median of sportsbook lines such as 3 and 3.5
  can be 3.25. Affected games remain in training and their medians remain in
  point-error comparisons; the medians are excluded from whole/half-point probability checks.
  The report states how many lines were excluded, so this no longer aborts
  a completed fit before the Apply button appears. Live quote validation and
  the original model architecture are unchanged.
- A restored CFB fit is distinguished from the separate raw history dataset.
  Save **Export checked historical dataset** before updating or closing the
  app. After applying a qualifying fit, save **Export model inputs and settings**.
  Restore those two files next time to reuse history and the applied fit.

## NFL additions retained from Prototype 08

- **NFL-only add-on profile**, active by default with individual toggles:
  passing EPA at 2.5× rushing, smoothed success, dated DVOA, non-blitz pressure,
  line yards, sustained-wind constraints, dynamic rest and confirmed bye bonus,
  division spread compression/lower totals and underdog ranking preference.
  The original score model and fitted feature architecture are unchanged.
- **Anytime Touchdown Scorers**, a separate section in the NFL tab: market-total
  threshold 47.5; high-volume RB1/2, WR1/2 and TE1; last-active-game bounce-back
  and sell-high rules that skip byes; red-zone shares, xTD versus actual TDs and
  opposing defensive red-zone DVOA. Refresh automatically builds player logs,
  actual snaps, usage ranks and a labeled xTD proxy, and attempts ATTD prices.
- NFL refresh tries actual pressure/blitz charting and exposed-venue sustained
  wind forecasts. Unavailable official DVOA/red-zone DVOA/ALY inputs have a
  dated provider import; missing terms are visibly skipped. Automatic line
  yards are labeled as an unadjusted PBP proxy. See [nfl_layers_guide.md](nfl_layers_guide.md)
  for the conceptual logic, callable pandas/numpy functions, schemas and exact
  bye-skipping example. These add-on weights have not been validated by the
  existing NFL reference fit.

## Retained from Prototype 07

- **NFL Historical Training**: free 2016–2025 downloads, CSV/CSV.gz/JSON/ZIP
  imports, compact per-game summaries, chronological gradient boosting,
  probability checks, export/restore, and a separate NFL CLV ledger.
- NFL inputs now include offensive/defensive EPA, dropback and designed-rush EPA,
  completion percentage over expected, sacks allowed/forced, and rest days.
  EPA/rest direct scenario weights begin at zero; they enter trained features.
- Included checked NFL history and a reference model/report. Under **NFL
  Historical Training → Included NFL history and reference fit**, load the
  history and fit results, review the report, then apply the qualifying target.
- The college profile emphasizes offense (65% offense / 35% opposing defensive
  allowances), full efficiency strength, FEI blend 40%, ranked-opponent letdown
  −2 margin points, and ranked-opponent look-ahead −2, capped at 3 combined.
  These are editable user preferences, not empirically fitted effects.
- College top-pick controls let you prioritize opponents in those spots, avoid
  flagged games, or rank every eligible game by conditional EV. All games remain
  visible. Missing AP snapshots leave automatic ranked flags unknown.
- **ATS and total trend finder** in both slate tabs: fixed screens for spread
  ranges, home/road, previous win/loss, road-loss return home, ranked-opponent
  situations, and prior offensive production/efficiency. Earlier games choose
  a back/fade or over/under direction; later games test it. Sample sizes,
  missing lines and a multiple-pattern confidence adjustment are shown.

## NFL reference fit: actual evaluation

The included regular-season dataset contains 2,639 games and 5,278 team game
summaries. One 2019 game has incomplete core play statistics. The feature
builder retains 2,290 eligible rows: 1,570 training (2016–2022), 480 validation
(2023–2024), and 240 untouched test games (2025). The first two weeks and
incomplete prior histories are excluded.

| Target | Validation baseline RMSE | Validation selected RMSE | 2025 selected RMSE | 2025 closing-market RMSE |
| --- | ---: | ---: | ---: | ---: |
| Away winning margin | 13.50 | 13.50 | 13.36 | 12.57 |
| Combined score | 13.88 | 13.42 | 13.50 | 12.97 |

The total correction qualifies on validation. The spread correction does not,
so it remains disabled. Both targets have larger 2025 errors than the historical
closing-market benchmark. This is a checked starting model, not demonstrated
profitable performance. Provider EPA/statistic revisions are retrospective;
these are reconstructed prior-week forecasts, not archived decision-time odds.

Historical closing lines are excluded from the trained prediction features.
The trend tables use historical reference lines and assumed −110 prices unless
you choose another price; their reported returns are illustrative. Trend
screens do not automatically add points to projections.

## What is included

- Separate **College Football** and **NFL** slate tabs, each with two top
  conditional-EV signals, a searchable table of every game, and score breakdowns.
- Independent league controls. Moving a CFB weight cannot change NFL scores.
  The sidebar explicitly identifies the league being edited. CFB look-ahead,
  major-win and return-home controls are scoped to College Football.
- Colley win/loss rankings and standard Massey least-squares point ratings from
  eligible prior results. Massey is the default CFB point-rating method;
  regularized result ratings remain selectable. Colley enters the trained
  gradient-boosting feature set rather than being mistaken for score points.
- **CFB Historical Training**: multiple CSV/JSON/ZIP uploads, checked coverage,
  cached API downloads, prior-week feature construction, two actual gradient
  boosting regressors, chronological train/validation/test splits and safe
  JSON model export/import. This release uses scikit-learn Gradient Boosting;
   it does not require XGBoost.
- Injury CSV/JSON/text/readable-PDF imports in each league tab. Review player
  status, actual report publication time and your point-impact assumption,
  then apply eligible reports. No universal automatic FBS injury feed is claimed.
- **Calibration & CLV**: held-out score/probability diagnostics, pregame archives,
  top-two quote logging and same-book closing-snapshot comparisons.
- Existing FEI, roster/transfer continuity, historical meetings, ten-year venue
  ATS records, explicit schedule/odds refresh and plain-English control help.

## Where to upload history

Open **CFB Historical Training → Upload historical CFB files**. Select all your
CSV/JSON files or one ZIP. Click **Import and check historical files**.

| Required export | CFBD exporter choice | Contents |
| --- | --- | --- |
| Games/results | Games → Returns historical game data | ID, season/week, teams, kickoff, venue, scores |
| Team game stats | Games → Returns team box score statistics by game | Both teams’ yards, passing/rushing attempts, turnovers |
| Advanced game stats | Stats → Returns advanced team statistics aggregated by game | ID/team; offensive/defensive plays, success rate, explosiveness |
| Optional lines | Betting → Returns betting lines | Provider, teams, formatted spread and total |

Use **game-level** statistics, not whole-season averages. Keep game IDs and
season/week/date fields. Nested CSV columns must contain readable JSON;
`[object Object]` cannot be reconstructed. Use direct API JSON if the CSV loses
nested fields. The checked dataset export can be uploaded here again.

## Does the API key already provide history?

The key authorizes requests to historical CFBD endpoints. Entering it alone does
not download ten years, create training rows, or fit the model. This week’s
refresh fetches current features and prior results/ATS history, which is a
separate process from historical gradient-boosting training.

Use **CFB Historical Training → Fetch history with my CFBD API key** to download
selected seasons. Start with one year, check coverage, then add the remaining
2016–2025 seasons. API requests are explicit, cache past responses locally,
and stop on provider errors while retaining earlier fetched files. Each season
uses three season requests including AP polls, optional lines, and one team-box request per regular
season week. The dashboard displays a request-budget estimate before fetching.

The [current free plan](https://collegefootballdata.com/api-tiers), checked
October 10, 2026, includes historical games, team statistics, advanced metrics
and lines with 1,000 calls/month; the free academic plan offers 3,000 calls/month
for registration with a `.edu` email. Access limits and field coverage can change.

## Train and apply

The default split is training before 2023, validation in 2023–2024, test in 2025
onward. At least 200 eligible training, 100 validation and 100 test games are
required. Every target game uses only completed earlier weeks from the same
season; the first two weeks and incomplete team histories are withheld.

Click **Build prior-week features and train CFB gradient boosting**. Review
MAE/RMSE, bias, test-market comparison, win/push/loss Brier score, log loss and
reliability bins. Apply only targets that improve validation RMSE. The validation
residual spread estimates uncertainty; final test outcomes do not choose trees,
correction caps or uncertainty. Changing core scoring controls disables the
trained correction until original controls are restored or training is repeated.

These exports reconstruct a prior-week statistical core retrospectively.
Provider revisions and historical FEI/roster/injury availability are not audited
as originally published at kickoff. Optional live modifiers remain separate
assumptions; the test report does not certify the full modified live model.
No historical training files or fitted model are included in this package.

## Injuries

Open a league slate → **injury reports and reviewed point impacts**. Upload a
CSV, JSON, text file or text-based PDF. A single-team PDF can use the selected
team; ambiguous rows require review. Scanned PDFs need OCR first.

Required columns: `team,player,status,report_as_of,point_impact`. Publication
needs a timezone, e.g. `2026-10-09T18:00:00-04:00`. Allowed statuses are Out,
Doubtful, Questionable, Probable and Available. A point impact is a reduction
in the player’s own team score, not an automatically learned player value.

Save reviewed rows, then apply them. Only the latest player status published
before kickoff and within 96 hours of it counts. Available cancels the player’s
impact. Effects cap at −10 score points per team. Missing reports mean unknown
coverage; they never certify that a team is healthy. A refresh preserves reviewed
reports and reapplies eligible records. NFL imports stay separate from CFB.

[Official SEC reports](https://www.secsports.com/fbreports) are one possible
source; conference and team coverage varies.

## CLV

Open **Calibration & CLV**. Log the current top two CFB signals before kickoff.
Only named-book quotes with actual update times less than six hours old qualify.
Download the CSV template and import the same book’s closing quotes using
`game_id,book,market,side,line,odds,quoted_at`.

Closing snapshots must be after capture, before kickoff and within the final
60 minutes. The dashboard reports how many minutes before kickoff the quote
was observed; it does not claim this was the exact last market update.

- Spread CLV = recorded handicap − closing handicap.
- Over CLV = closing total − recorded total.
- Under CLV = recorded total − closing total.

Positive means a more favorable recorded line. Price CLV is comparable only
at the same line and includes bookmaker margin. Closing prices are not model
features. General historical CFBD lines are never automatically called closing
lines. CLV is a monitoring measure, not proof of profitable returns.

## Save your work

Streamlit sessions and local caches can disappear after restart/redeployment.
Download and retain:

1. **Export checked historical dataset** for the large historical inputs.
2. **Export model inputs and settings** in Data & setup for trained corrections,
   parameters, slate, reviewed injuries, manual inputs and CLV records.
3. **Export CLV ledger**, **Download trained CFB model** and backtest rows as
   separate convenient backups. Raw source files should also be retained.

Imports stay in the session until you export them; this release has no permanent
database or scheduled refresh service.

## Refresh weekly and protect keys

In **Data & setup**, choose the season, dates and leagues, supply keys, then
click **Refresh schedules, statistics and odds**. Sliders reuse loaded data.
NFL uses public nflverse schedules and prior-week play-by-play. CFBD supplies
CFB regular-season games involving FBS teams, prior-week stats and historical
results. The Odds API supplies each book’s quoted spread/total and price.
Postseason and FCS-only slates are outside the current adapters.

Set these in Streamlit Secrets, or use password fields in the app:

```toml
CFBD_API_KEY = "your key"
ODDS_API_KEY = "your key"
```

Never commit real keys. Keys are not included in exported datasets, snapshots,
trained models or the ZIP. The multi-book median is a reference; conditional
EV uses each individual quote’s actual line and odds. Zero context weights or
missing optional data skip their terms. Rankings do not force favorites or
disguise spread-cover predictions as outright-win predictions.

## Run locally

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m streamlit run app.py
```

On Windows use `.venv\Scripts\Activate.ps1` to activate instead. Streamlit
respects its selected light/dark theme. Place `.streamlit/config.toml` at the
GitHub repository root for Community Cloud, even with a nested app entrypoint.
A `.py` script is not a Colab `.ipynb` notebook.

## Verification and model scope

```bash
python -m unittest discover -s tests -v
```

Tests cover scoring, push/price arithmetic, prior-week chronology, Colley and
Massey units, disconnected schedules, import conflicts and round trips, API
request structure, safe tree inference, test-selection independence, league
isolation, injury freshness/latest status and CLV signs/timing. See
`training_guide.md` for the training definitions and limitations.

The app begins with a frozen 11-game October 10, 2026 example and the editable
college preference profile. Refresh the slate to load current games and odds.
The NFL reference dataset and fit can be loaded explicitly in NFL Historical Training.
Authenticated CFBD/Odds downloads require your keys; development checks use
provider-schema fixtures. No model can guarantee the best predictions.
Normal residuals approximate football scoring and require ongoing calibration.
