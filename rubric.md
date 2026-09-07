# Capstone rubric — batch edition

Total 100, plus extra credit. The sections follow the lectures you were
given — Day 2's floor and the rolling-origin harness, and Day 3's
orchestration through AutoGluon — made checkable. Grade the work that is
there; a defensible wrong answer scores higher than an undefendable right
one.

## A — The floor (25)

| point | criterion |
|---|---|
| 8 | Fits the seasonal naive (the floor) and at least one other benchmark |
| 7 | Ljung-Box on the floor's residuals (`y_t - y_{t-12}`), with a sentence on what structure is left on the table and which kind of model eats it |
| 6 | Gives the floor an honest interval and reads its 80% coverage against the nominal 80% |
| 4 | The calendar check: months, range, gaps — confirmed, not assumed |

## B — The framework (20)

| point | criterion |
|---|---|
| 5 | The data conversion to AutoGluon's frame is done by hand and visible — not hidden behind a helper |
| 6 | Fits a real zoo (more than one model), and the leaderboard is displayed on the held-out months |
| 4 | Reads the leaderboard: names the **sign** of `score_val`, which models it fit, and how many windows it scored them on |
| 5 | Names the model the leaderboard shortlists as the thing to cross-validate next, and says in one sentence what that ranking can and cannot be trusted to do |

## C — The harness (25)

| point | criterion |
|---|---|
| 8 | Cross-validates the shortlisted model over the SAME rolling origins (8 windows, h=12, step=12) as the floor — through the harness, not one holdout |
| 7 | One table, one row per model: MASE, scaled CRPS, and 80% coverage; reads each model against the floor |
| 5 | Reports where the model beats the floor and where it loses, on which metric — a win on the point and a loss on the distribution is reported as both |
| 5 | The coverage verdict: which band is too wide, too narrow, or honest, and what that says about the model's uncertainty |

## D — The report (30)

| point | criterion |
|---|---|
| 8 | The one-sentence recommendation, and the one number that earns it |
| 7 | The intervals: width, coverage, and a verdict on honesty |
| 6 | The residuals: what the model missed, and what that suggests it is missing |
| 5 | One specific, credible next step |
| 4 | Written for a manager: no unexplained jargon, charts that carry their claims, length actually kept |

## Anti-patterns (deductions, stack)

- MASE reported without CRPS or coverage — the number is half a result (−5)
- One holdout window (including AutoGluon's own single split) presented as
  evidence for a ranking (−10)
- No benchmark floor, or the floor present but not compared (−10)
- No intervals anywhere in the notebook or the report (−10)
- A framework result quoted without saying what it fits or how it ranked
  internally (−5)
- Leakage of any kind: a fit or a denominator touching data the fold was
  not allowed to see (−15, and flag it to the student)

## Extra credit (10 points)

- Dynamic regression on a driver you source and cache (e.g. monthly mean
  temperature from a public weather archive), with the cross-validation
  discipline applied to the regression's residuals.
