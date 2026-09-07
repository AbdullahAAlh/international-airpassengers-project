# Capstone rubric

Total 100, plus extra credit. The four sections are the course spine, made
checkable. Grade the work that is there; a defensible wrong answer scores
higher than an undefendable right one.

## A — Exploration and shape (20)

| point | criterion |
|---|---|
| 5 | Plots the series and identifies trend and seasonality in plain language, not just "it has trend and seasonality" |
| 5 | STL decomposition; reads all three panels and says which component does the work |
| 5 | Trend and seasonal strength computed (textbook formulas), with a sentence on what they imply for model choice |
| 5 | ACF of the raw series; knows what the lag-12 spike means; states the shape in one or two sentences a non-forecaster could repeat |

## B — Floor and evaluation (30)

| point | criterion |
|---|---|
| 8 | Fits the seasonal naive (the floor) and at least one other benchmark |
| 8 | Cross-validates over rolling origins (8 windows, h=12, step=12) through the harness — not one holdout |
| 7 | Reports per model: MASE, scaled CRPS, and 80% coverage; reads coverage against the nominal 80% and says what the gap means |
| 7 | Ljung-Box on the floor's residuals, with a sentence on what structure is left on the table and which kind of model eats it |

## C — Your model (25)

| point | criterion |
|---|---|
| 8 | Fits ETS and reads it back: which (E,T,S) variant the search chose, what each letter says, and whether it matches the section-1 read |
| 9 | Cross-validates on the SAME folds with the same harness and levels; compares against the floor on the point forecast AND the distribution (a win on one and a loss on the other is reported as both) |
| 8 | If any black-box tool is used: names what it picked and says in one sentence why its output is or is not readable |

## D — The report (25)

| point | criterion |
|---|---|
| 7 | The one-sentence recommendation, and the evidence behind it |
| 6 | The intervals: width, coverage, and a verdict on honesty |
| 6 | The residuals: what the model missed, and what that suggests it is missing |
| 4 | One specific, credible next step |
| 2 | Written for a manager: no unexplained jargon, charts that carry their claims, length actually kept |

## Anti-patterns (deductions, stack)

- MASE reported without CRPS or coverage — the number is half a result (−5)
- One holdout window presented as evidence for a ranking (−10)
- No benchmark floor, or the floor present but not compared (−10)
- No intervals anywhere in the notebook or the report (−10)
- A framework result quoted without saying what it fits or how it ranked
  internally (−5)
- Leakage of any kind: a feature, a fit, or a denominator touching data the
  fold was not allowed to see (−15, and flag it to the student)

## Extra credit (10 points each)

- Dynamic regression on a driver you source and cache (e.g. monthly mean
  temperature from a public weather archive), with the cross-validation
  discipline applied to the regression's residuals (10)
- AutoGluon on the same series: what does its internal leaderboard claim,
  and does it survive your harness? (10)
