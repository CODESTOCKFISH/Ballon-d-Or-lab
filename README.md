# Ballon d’Or Lab

An interactive, fan-made Ballon d’Or ranking model for exploring how different criteria and priorities can change the order of 30 nominees.

**Live site:** https://codestockfish.github.io/Ballon-d-Or-lab/  
**Repository:** https://github.com/CODESTOCKFISH/Ballon-d-Or-lab

> **Important:** This is an independent analytical project, not an official Ballon d’Or prediction. Several component ratings are provisional judgement scores, and some supporting data still needs consistent verification. The ranking should be read as a transparent model output—not as objective fact.

## What the app does

- Ranks 30 nominees using an editable weighted score.
- Lets users adjust the importance of each scoring criterion.
- Shows score contributions and supports player profiles and head-to-head comparisons.
- Compares alternative weighting scenarios: the default formula, World Cup priority, club-season priority, and trophy priority.
- Includes a data-audit view to distinguish sourced notes from information that needs verification.
- Displays FotMob ratings as contextual tie-break information only; they do **not** enter the core score.
- Shows World Cup knockout appearances evidenced in user-supplied lineup/bench screenshots. These counts are limited to the screenshots and are **not** full-season availability or a complete tournament log.
- Exports the current ranking and relevant data fields to CSV.
- Includes an optional, source-linked availability calculator. Valid entries are stored in the browser and can affect that browser’s ranking.

## Evaluation window

**3 August 2025 – 19 July 2026**

The intended comparison is season-wide club and senior international performance within this window. When entering data, totals must use consistent competition scope and dates; for example, do not compare league-only minutes for one player with all-competition minutes for another.

## Default scoring model

The default weights are:

| Criterion | Weight | What it is intended to capture |
|---|---:|---|
| Club performance | 23.0% | Season contribution using role-appropriate production, chance creation, progression, and defensive work |
| Trophies | 15.9% | Importance of team achievements, with meaningful player participation considered |
| International performance | 19.8% | National-team and tournament contribution, with substantial World Cup emphasis |
| Individual awards | 13.5% | Major individual recognition, used carefully to avoid double-counting performance already captured elsewhere |
| Clutch / knockout impact | 11.1% | Verified decisive actions in high-stakes matches, considering round and match context |
| Availability & consistency | 16.7% | Minutes and appearances across the evaluation window, when comparable verified inputs are available |
| **Total** | **100.0%** | |

The weights are adjustable in the app. The alternative scenario presets deliberately change the emphasis to test how rankings respond to different philosophies; they are sensitivity checks, not claims that one philosophy is universally correct.

## Formulas

### 1. Weighted score with missing-data renormalisation

Each criterion has a rating on a 0–100 scale. The app calculates a weighted average using only criteria with numeric values for that player. If a criterion is missing, the remaining available weights are renormalised rather than treating missing data as zero.

Let:

- \(x_i\) be a player’s score for criterion \(i\),
- \(w_i\) be the current weight for criterion \(i\),
- \(K\) be the set of criteria with numeric scores for that player.

The score is:

\[
S = \frac{\sum_{i\in K} x_i w_i}{\sum_{i\in K} w_i}
\]

If all six criteria are present and the weights sum to 100, this is the usual weighted average. If availability is missing, for example, the app does not silently give that player an availability score of zero; it calculates the score from the available criteria and their renormalised weights. Consequently, players can have different data coverage, which is why the app flags uncertainty.

### 2. Availability & consistency score

When a user enters valid, source-linked totals, availability is calculated as:

\[
A = 100\left(0.70\frac{M}{M_p} + 0.30\frac{P}{P_p}\right)
\]

Where:

- \(M\) = minutes played
- \(M_p\) = possible minutes in the chosen scope
- \(P\) = appearances
- \(P_p\) = possible team appearances in the same scope

Minutes availability contributes **70%** and appearance availability contributes **30%**. Inputs are rejected if denominators are not positive, totals are negative, or played totals exceed possible totals. The result is shown on a 0–100 scale to one decimal place.

**Availability is not pre-populated with a verified dataset in the published build.** Until valid totals and a source URL are entered for a player, that criterion is excluded for that player. Entries are saved in the browser’s local storage, so they do not automatically sync across devices or visitors and are not embedded into the public website’s data.

## Other modelling choices

- **Domestic league goal multiplier (methodology rule):** the project’s intended scoring guidance gives domestic league goals a multiplier of **2.0×** in the top five European leagues (Premier League, La Liga, Bundesliga, Ligue 1, Serie A) and **1.5×** in other domestic leagues. This is a scoring-design rule, not a live raw-stat calculation performed by the current HTML app: the app currently ranks the component ratings already stored in its player data.
- **FotMob rating:** shown for context and tie-break exploration only; it does not contribute to the weighted score.
- **Big chances created:** intended as tie-break/context evidence rather than a core score by itself. Comparable big-chance scoring data may inform core evaluation only when scope is consistent.
- **Prediction-market overlay:** disabled and excluded.
- **Nomination history:** excluded.
- **Knockout appearances:** the visible column counts only appearances supported by the lineup/bench screenshots supplied for the project. It is not used as a substitute for a complete match log or availability data.
- **No fabricated neutral availability:** missing availability remains unknown instead of receiving an invented score.

## Data quality and limitations

The app is designed to make assumptions visible, but it is not yet a fully audited statistical model:

1. Some core component ratings are provisional judgement ratings carried over from the initial scoring sheet.
2. Some values use neutral placeholders where evidence was not fully assessed. A placeholder is not a measured performance result.
3. Research notes can differ in date range or competition scope. Source links should be checked against the exact evaluation window and the same all-competition/league-only definition before treating a number as verified.
4. The supplied knockout screenshots cover only the matches visible in those images; they cannot establish a complete season or tournament record.
5. Availability affects the ranking only after valid inputs are entered. Browser-local entries do not update the public dataset for everyone.
6. The model is sensitive to chosen weights. The scenario tools are included to make that sensitivity inspectable rather than hiding it.

## How to use

1. Open the [live app](https://codestockfish.github.io/Ballon-d-Or-lab/).
2. Explore the default ranking and adjust the criteria weights.
3. Select a player to inspect their score, evidence notes, and data status.
4. Use player comparison and scenario views to understand why positions change.
5. Enter availability totals only when you have a reliable source and consistent scope for the evaluation window.
6. Export the current ranking to CSV if you want to inspect or analyse the results elsewhere.

## Technology

The project is a self-contained HTML application with CSS and JavaScript embedded in `index.html`. GitHub Pages serves the static file; no server-side database is required for the basic experience. Availability entries are stored locally in the browser.

## Updating the site

After editing `index.html` or `README.md` in the local repository, run:

```powershell
git add index.html README.md
git commit -m "Update Ballon d'Or Lab"
git push
```

GitHub Pages will publish the committed changes after its deployment workflow completes.

## Credits and disclaimer

Built as an independent football analytics and decision-modelling project. Research links and user-supplied lineup screenshots inform some evidence notes; they should be reviewed at source before reuse. Player names, team references, ratings, and model outputs are presented for analysis and discussion. This project is not affiliated with or endorsed by France Football, the Ballon d’Or, FIFA, FotMob, or any football club or league.
