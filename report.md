# International Air Passengers Forecasting Report

## Executive Summary

The international air passenger series shows a strong upward trend together with a clear annual seasonal pattern. Passenger numbers rise substantially over the 1949–1960 period, while similar peaks and troughs repeat each year. The size of those seasonal fluctuations also becomes larger as the overall passenger level rises, so a useful forecasting model needs to represent both long-term growth and annual seasonality.

**I recommend AutoARIMA as the forecasting model because its rolling-origin MASE was 0.647, compared with 1.313 for the Seasonal Naive benchmark.**

This recommendation is based on eight rolling-origin evaluation windows, each forecasting 12 months ahead. The same evaluation setup was applied to the benchmark and the shortlisted model, which makes the comparison more reliable than relying on a single holdout period.

AutoARIMA also performed better on the other main evaluation measures. Its RMSSE was 0.683 compared with 1.246 for Seasonal Naive, and its scaled CRPS was 0.0339 compared with 0.0616. These results indicate that AutoARIMA improved both the accuracy of its central forecasts and the quality of its probabilistic forecasts.

A simple Naive model was also included as an additional benchmark. It produced a MASE of 2.119, which was considerably worse than both Seasonal Naive and AutoARIMA. This confirms that simply carrying the most recent passenger value forward is not appropriate for a series with such strong annual seasonality.

## What the Data Shows

Before fitting any model, the monthly calendar was checked. The dataset contains 144 monthly observations from January 1949 through December 1960 with no missing months. This is important because missing timestamps could distort seasonal relationships and lead to misleading forecasts.

The exploratory analysis shows two dominant features. First, there is a strong upward trend throughout the series. Second, there is a clear annual pattern in which similar seasonal movements repeat every 12 months. The seasonal movement becomes wider as passenger levels increase, indicating that the amount of seasonal variation is not constant over time.

The STL decomposition confirms these observations. Trend strength was measured at 1.00 and seasonal strength at 0.99, indicating that both components are extremely strong. The remainder was mostly centered around zero, although several larger unexplained movements remained, particularly toward the later part of the series.

The autocorrelation analysis also supports this structure. Correlation remained positive across many lags and declined slowly, which is consistent with a persistent trend. The autocorrelation was approximately 0.76 at lag 12 and 0.53 at lag 24. In practical terms, passenger numbers remain strongly related to the same month one year earlier and even retain noticeable dependence across two years.

## Benchmark Performance

The Seasonal Naive model was used as the main forecasting floor. This model predicts each future month using the observed value from the same month one year earlier. It is therefore a strong basic benchmark for monthly seasonal data because it automatically preserves the annual pattern.

However, the benchmark did not capture all of the systematic structure in the series. Its in-sample residuals were tested using the Ljung-Box diagnostic at lags 12 and 24. The resulting p-values were effectively zero, indicating that the remaining errors were not random. In other words, repeating last year's monthly values preserved the annual seasonal pattern but left predictable structure unexplained.

This is consistent with the visual pattern of the data. The series is not only seasonal; it also has continuing growth and a seasonal amplitude that changes with the level. A model that can adapt to changing time dependence therefore has an opportunity to improve on the Seasonal Naive floor.

The framework initially compared several forecasting approaches: Naive, SeasonalNaive, AutoETS, AutoARIMA, and Theta. Theta failed for this series during the framework fit and used SeasonalNaive as a fallback. The framework's internal ranking was based on only one validation window, so it was treated only as a shortlist rather than final evidence. AutoARIMA performed best on the held-out test period and was therefore selected for the full rolling-origin evaluation.

## Rolling-Origin Evaluation

The final model comparison used eight rolling-origin windows with a 12-month forecast horizon and a 12-month step. This means each model was repeatedly asked to forecast the following year from different historical cutoff points.

The results were:

| Model | MASE | RMSSE | Scaled CRPS | 80% Coverage |
|---|---:|---:|---:|---:|
| Naive | 2.119 | 2.452 | 0.1103 | 68.8% |
| Seasonal Naive | 1.313 | 1.246 | 0.0616 | 51.0% |
| AutoARIMA | **0.647** | **0.683** | **0.0339** | **69.8%** |

Lower values are preferred for MASE, RMSSE, and scaled CRPS. AutoARIMA achieved the lowest value on all three measures. Its MASE of 0.647 was roughly half the Seasonal Naive value of 1.313, providing the strongest evidence for recommending it.

The additional Naive benchmark performed poorly on forecast accuracy despite achieving 68.8% interval coverage. This shows why coverage alone cannot be used to choose a model. A model can generate a relatively broad interval that contains many observations while still producing weak central forecasts.

## Prediction Intervals

Prediction intervals are important because a forecast should communicate uncertainty rather than provide only one future value.

Across the rolling-origin evaluation, the average width of the 80% prediction interval was **77.758 thousand passengers for Seasonal Naive** and **44.076 thousand passengers for AutoARIMA**. AutoARIMA therefore produced substantially narrower intervals.

At the same time, AutoARIMA's 80% intervals covered **69.8%** of the observed values, compared with only **51.0%** for Seasonal Naive. AutoARIMA therefore provided a better balance between useful interval width and actual coverage.

However, the AutoARIMA intervals should not be considered fully calibrated. An interval labeled as 80% should ideally contain close to 80% of future observations under repeated use. The observed coverage of 69.8% is below that target. This means the AutoARIMA intervals are still somewhat too narrow and are more confident than the evaluation results justify.

The scaled CRPS result supports the same overall conclusion. AutoARIMA achieved 0.0339 compared with 0.0616 for Seasonal Naive, indicating stronger probabilistic forecast performance even though its interval calibration is not yet ideal.

## Residual Diagnostics

AutoARIMA reduced forecast errors substantially, but it did not remove all systematic dependence.

Using the rolling-origin forecast errors, the Ljung-Box p-values were approximately 5.71 × 10^-26 at lag 12 and 1.34 × 10^-37 at lag 24. These extremely small values indicate that the AutoARIMA errors are not white noise. Some recurring time dependence therefore remains after fitting the recommended model.

This is an important limitation. AutoARIMA is clearly better than the benchmarks according to the rolling-origin accuracy measures, but the residual diagnostic shows that it has not captured every repeatable feature of the passenger series. The remaining structure may be related to the changing size of the seasonal fluctuations as passenger levels increase.

For a manager, the practical conclusion is that AutoARIMA is the strongest model evaluated here, but its forecasts should still be monitored. The residual structure and below-target interval coverage both suggest that there is room to improve uncertainty estimates and capture additional systematic behavior.

## Recommended Next Step

The next change I would test is applying a logarithmic transformation to the passenger series before fitting and evaluating the forecasting model.

The exploratory plots show that seasonal fluctuations become larger as passenger levels rise. A logarithmic transformation is designed to stabilize this type of increasing variation by making proportional changes more comparable across low and high passenger levels.

I would fit the transformed model using exactly the same eight rolling-origin windows, 12-month horizon, and evaluation metrics used in this project. The transformed approach should only replace the current AutoARIMA recommendation if it produces lower MASE and scaled CRPS while also moving the 80% prediction-interval coverage closer to the intended 80%.

The main objective of this next step would therefore not be complexity for its own sake. It would be to reduce the systematic structure still present in the residuals and produce prediction intervals that better reflect the actual uncertainty in future passenger demand.

## Conclusion

The evaluation supports AutoARIMA as the preferred forecasting model for this dataset. It substantially outperformed both Naive and Seasonal Naive on rolling-origin forecast accuracy and produced better probabilistic forecasts.

The recommendation is not based on a single holdout result. It is supported by eight repeated 12-month forecasting windows, where AutoARIMA achieved a MASE of 0.647 versus 1.313 for Seasonal Naive.

At the same time, the model is not perfect. Its 80% prediction intervals covered only 69.8% of observations, and its residuals still showed significant serial dependence. AutoARIMA should therefore be used as the current forecasting choice while further work focuses on stabilizing the changing seasonal variation and improving interval calibration.
