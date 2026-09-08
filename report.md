# International Air Passengers Forecasting Report

**Prepared by Abdullah Abdulaziz Alhomaidan**

**Course Final Project — Time Series Analysis & Forecasting**

## Executive Summary

The international air passenger series shows a strong upward trend and a clear annual seasonal pattern. The seasonal fluctuations also increase as passenger levels rise, indicating that the variability changes with the level of the series.

Based on the rolling-origin evaluation, I recommend **LogAutoARIMA** as the final forecasting model. It achieved a MASE of **0.638**, compared with **0.647** for AutoARIMA and **1.313** for the Seasonal Naive benchmark.

The recommendation is based on eight rolling-origin evaluation windows, each forecasting 12 months ahead. LogAutoARIMA also achieved the lowest RMSSE and scaled CRPS and provided better 80% interval coverage than the other leading models.

## What the Data Shows

The dataset contains **144 monthly observations** from January 1949 through December 1960, with no missing months.

The time-series plot shows a strong long-term upward trend and a repeating annual seasonal pattern. Importantly, the seasonal fluctuations become larger as passenger levels increase. This suggests multiplicative behavior and provides a strong motivation for applying a logarithmic transformation.

The STL decomposition confirms the strength of the observed structure. Trend strength was **1.00** and seasonal strength was **0.99**, showing that both components are extremely strong.

The ACF also remains strongly positive across many lags. The autocorrelation is approximately **0.76 at lag 12** and **0.53 at lag 24**, confirming persistent annual seasonality together with strong time dependence.

## Benchmark and Model Selection

The Seasonal Naive model was used as the main forecasting floor. It predicts each future month using the value observed in the same month one year earlier.

A simple Naive model was also included as an additional benchmark. The rolling-origin evaluation produced a MASE of **2.119** for Naive and **1.313** for Seasonal Naive, confirming that preserving annual seasonality is substantially better than simply carrying the most recent value forward.

The Seasonal Naive residuals were not white noise. Ljung-Box tests at lags 12 and 24 produced extremely small p-values, indicating that substantial predictable structure remained after applying the benchmark.

The AutoGluon framework compared Naive, SeasonalNaive, AutoETS, AutoARIMA, and Theta. Theta failed for this series and used SeasonalNaive as a fallback. Because the framework ranking was based on only one validation window, it was treated as a shortlist rather than final evidence.

AutoARIMA performed best on the held-out test period and was therefore taken forward to the rolling-origin evaluation.

Because the exploratory analysis showed that seasonal variation increased with the passenger level, I also evaluated AutoARIMA after applying a logarithmic transformation. This produced the final **LogAutoARIMA** model.

## Rolling-Origin Evaluation

All final models were evaluated using the same eight rolling-origin windows, with a 12-month forecast horizon and a 12-month step.

| Model | MASE | RMSSE | Scaled CRPS | 80% Coverage | Worst-fold MASE |
|---|---:|---:|---:|---:|---:|
| Naive | 2.119 | 2.452 | 0.1103 | 68.8% | 3.196 |
| Seasonal Naive | 1.313 | 1.246 | 0.0616 | 51.0% | 1.959 |
| AutoARIMA | 0.647 | 0.683 | 0.0339 | 69.8% | 1.594 |
| **LogAutoARIMA** | **0.638** | **0.653** | **0.0327** | **72.9%** | **1.173** |

Lower values are preferred for MASE, RMSSE, and scaled CRPS. LogAutoARIMA achieved the best result on all three measures.

It also reduced the worst-fold MASE from **1.594** for AutoARIMA to **1.173**, showing that its improvement was not limited to the average result; it also performed more consistently across the different historical forecast origins.

These results confirm that the logarithmic transformation is useful for this series and that LogAutoARIMA is the strongest model evaluated.

## Prediction Intervals

Prediction intervals communicate the uncertainty around the forecast rather than providing only a single future value.

The average width of the **80% LogAutoARIMA prediction interval was 53.673 thousand passengers**. Its intervals covered **72.9%** of the observed values across the rolling-origin evaluation.

For comparison, Seasonal Naive produced an average 80% interval width of **77.758 thousand passengers** while covering only **51.0%** of the observations.

The original AutoARIMA model produced narrower intervals, with an average width of **44.076 thousand passengers**, but its coverage was lower at **69.8%**. The logarithmic transformation therefore slightly widened the intervals while improving their empirical coverage and forecast accuracy.

However, LogAutoARIMA's 72.9% coverage is still below the nominal 80% target. Its uncertainty bands therefore remain somewhat too narrow, although they are better calibrated than those of the other leading models.

## Residual Diagnostics

LogAutoARIMA substantially improved forecast accuracy, but some systematic dependence remains in its forecast errors.

The Ljung-Box p-values for the rolling-origin errors were approximately **3.37 × 10^-37 at lag 12** and **2.02 × 10^-47 at lag 24**. These values are far below conventional significance levels, indicating that the remaining errors are not white noise.

This means that LogAutoARIMA is the best model tested, but it has not captured every repeatable feature of the passenger series. The residual structure and below-target interval coverage both indicate that further improvement may still be possible.

## Recommended Next Step

The next improvement I would test is adding an external explanatory variable, such as **monthly temperature**, through a dynamic regression forecasting model.

An external driver may explain part of the systematic structure that remains after using historical passenger values alone. I would evaluate this model using exactly the same eight rolling-origin windows, 12-month forecast horizon, and evaluation metrics used in the current analysis.

The new model should only replace LogAutoARIMA if it reduces MASE and scaled CRPS while also bringing the 80% prediction-interval coverage closer to the intended 80% level.

## Conclusion

The final evaluation supports **LogAutoARIMA** as the preferred forecasting model for the international air passenger series.

It achieved the best overall rolling-origin results, with a MASE of **0.638**, RMSSE of **0.653**, scaled CRPS of **0.0327**, and 80% coverage of **72.9%**. It also achieved the lowest worst-fold MASE among the models evaluated.

The logarithmic transformation was particularly appropriate because the seasonal fluctuations increased as passenger levels rose. Although residual dependence remains and interval coverage is still below the ideal 80%, LogAutoARIMA currently provides the strongest combination of forecast accuracy, robustness, and uncertainty estimation among the evaluated models.
