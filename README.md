# Energy Consumption Forecasting

A time-series project that forecasts electricity, gas and water consumption.

The project explores trends and seasonality, compares forecasting models and produces **60-month forecasts** with uncertainty bands.

## Repository Files

| File | Description |
| --- | --- |
| [Energy Comsumption Forecast.ipynb](Energy%20Comsumption%20Forecast.ipynb) | Notebook containing the analysis, modelling code and saved results. |
| [Energy Consumption Forecasting.pptx](Energy%20Consumption%20Forecasting.pptx) | Presentation summarising the workflow and findings. |
| [Energy-Consumption-Data.csv](Energy-Consumption-Data.csv) | Dataset used for analysis and forecasting. |

## Dataset

The included dataset contains **397 monthly records**, from **January 1990 to January 2023**, with no missing values.

| Column | Description |
| --- | --- |
| `DATE` | Monthly observation date |
| `Gas Consumption (tons)` | Gas consumption |
| `Electricity Consumption (MWh)` | Electricity consumption |
| `Water Consumption (tons)` | Water consumption |

## Project Workflow

1. Parse dates and set the time-series index.
2. Explore consumption patterns and seasonal decomposition.
3. Split the data chronologically without shuffling.
4. Use the Augmented Dickey–Fuller (ADF) test to examine stationarity.
5. Compare Simple Exponential Smoothing, Holt–Winters and SARIMA.
6. Tune SARIMA configurations using validation MAPE.
7. Fit final forecasting models on the full history.
8. Generate forecasts for the next **60 months**.

## Data Split

| Set | Records | Period |
| --- | ---: | --- |
| Training | 277 | January 1990 – January 2013 |
| Validation | 60 | February 2013 – January 2018 |
| Test | 60 | February 2018 – January 2023 |

## Stationarity Analysis

ADF tests are performed on the training data.

| Series | ADF p-value |
| --- | ---: |
| Gas | < 0.0001 |
| Electricity | 0.7664 |
| Water | 0.0164 |
| Electricity after first differencing | 1.53 × 10⁻⁸ |

First differencing substantially reduces the electricity series' ADF p-value.

## Models Compared

- **Simple Exponential Smoothing:** models the series level.
- **Holt–Winters:** models level, trend and seasonality.
- **SARIMA:** models autoregressive, differencing and moving-average patterns with seasonality.

Seasonal models use a **12-month period**.

### Initial Model Results

The table shows test MAPE from the notebook's saved outputs. Lower values indicate smaller percentage errors.

| Model | Electricity | Gas | Water |
| --- | ---: | ---: | ---: |
| Simple Exponential Smoothing | 13.38% | 12.56% | 22.70% |
| Holt–Winters | 16.29% | 9.98% | 24.77% |
| Initial SARIMA | 12.35% | 12.70% | 17.87% |

## SARIMA Tuning

Configurations are ranked by validation MAPE.

| Series | Best Recorded Configuration | Validation MAPE |
| --- | --- | ---: |
| Electricity | `(1,1,0)(3,1,1,12)` | 4.03% |
| Gas | `(0,1,0)(2,1,0,12)` | 21.97% |
| Water | `(3,0,2)(3,1,1,12)` | 35.80% |

The notebook reports the following results in its tuned-model evaluation cells:

| Series | Reported MAPE | Reported MAE | Reported RMSE |
| --- | ---: | ---: | ---: |
| Electricity | 3.33% | 34.42 MWh | 44.39 MWh |
| Gas | 20.29% | 5.61 tons | 6.56 tons |
| Water | 19.97% | 91.15 tons | 111.65 tons |

These values are reproduced from the saved notebook outputs.

## Final Forecasts

The notebook fits models on the full dataset to forecast **February 2023 to January 2028**.

| Series | Full-history Forecast Model |
| --- | --- |
| Electricity | SARIMA `(1,1,0)(3,1,1,12)` |
| Gas | Holt–Winters with additive trend and seasonality |
| Water | SARIMA `(2,1,1)(2,1,1,12)` |

### Forecast Insights

- **Electricity:** upward trend with an annual seasonal pattern.
- **Gas:** mild growth with seasonal variation.
- **Water:** variable consumption with wide forecast uncertainty.

Electricity and water use model-generated 95% intervals. Gas uses an approximate residual-based band calculated as the forecast ± 1.96 residual standard deviations.

## Evaluation Metrics

- **MAPE:** mean absolute percentage error.
- **MAE:** mean absolute error in the consumption unit.
- **RMSE:** root mean squared error, which gives more weight to larger errors.

Lower values are better for all three metrics.

## Tools Used

- Python
- pandas and NumPy
- statsmodels
- scikit-learn
- Matplotlib and Seaborn
- Jupyter Notebook

## How to Run

1. Download or clone the repository.
2. Install the required packages:

   ```bash
   python -m pip install pandas numpy statsmodels scikit-learn matplotlib seaborn notebook
   ```

3. Keep `Energy-Consumption-Data.csv` beside the notebook.
4. Replace the existing Windows path and dataset filename in the loading cell with:

   ```python
   df = pd.read_csv("Energy-Consumption-Data.csv")
   ```

5. Start Jupyter:

   ```bash
   jupyter notebook
   ```

6. Open `Energy Comsumption Forecast.ipynb` and run the cells in order.

SARIMA parameter searches may take considerable time. Results can vary with package versions and model convergence.

## Future Improvements

- Add seasonal-naive forecasts as a reference baseline.
- Use rolling-origin validation to evaluate performance across several periods.
- Inspect residuals for remaining patterns.
- Test additional trend and seasonal configurations.
- Compare forecast intervals with observed outcomes as new data becomes available.

## Author

**Yan Myoe Naing**  
Applied AI and Analytics, Singapore Polytechnic
