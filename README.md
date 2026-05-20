# Crop Production Analytics & Fertilizer Demand Forecasting

An end-to-end data science project exploring agricultural production patterns to estimate regional fertilizer demand. Built using the CRISP-DM framework as part of a portfolio targeting the fertilizer and agriculture industry.

---

## About This Project

Fertilizer distribution is a real logistical challenge — in countries like Indonesia and India, subsidized fertilizer often doesn't reach the right regions at the right time, not because supply is insufficient, but because planning isn't data-driven.

India's crop production data was chosen as a proxy for Indonesia's context. Both countries run subsidized fertilizer programs, share two main planting seasons, rely heavily on rice and maize, and face similar regional distribution challenges. The methodology here can be directly adapted for Indonesian provincial data.

---

## Dataset

| | |
|---|---|
| Source | [Crop Production in India — Kaggle (abhinand05)](https://www.kaggle.com/datasets/abhinand05/crop-production-in-india) |
| Size | 246,091 rows × 7 columns |
| Period | 1997 – 2015 |
| Coverage | 646 districts · 33 states · 124 crop types · 6 seasons |

---

## Analyses

Four models, each answering a different business question:

**Clustering (K-Means)**
Which districts share similar farming profiles and fertilizer needs? 646 districts were grouped into 4 segments based on harvested area, yield efficiency, total production, and crop diversity.

**Classification (Random Forest)**
Can fertilizer demand level (High / Medium / Low) be predicted for a given district and season? A Random Forest was trained inside a scikit-learn Pipeline to prevent data leakage, evaluated with 5-fold cross-validation.

**Time Series (Prophet)**
How will rice fertilizer demand trend in coming years? Meta's Prophet was used to forecast demand based on 15 years of historical data, tested on 2013–2014.

**Geospatial (Folium)**
Where are high-demand and low-yield districts located? 652 districts were geocoded via OpenStreetMap and plotted as an interactive heatmap showing fertilizer demand intensity across India.

---

## Results

| Model | Metric | Score |
|---|---|---|
| K-Means Clustering | Silhouette Score | 0.398 |
| Random Forest | CV F1-Score (macro) | 0.978 ± 0.001 |
| Random Forest | Test F1-Score (macro) | 0.981 |
| Prophet | MAPE (test 2013–2014) | 6.38% |
| Geocoding | District coverage | 97.5% |

### Cluster Profiles

| Cluster | Label | Districts | Key Characteristics |
|---|---|---|---|
| 0 | Moderate Farming Zone | 358 | Medium-scale, average yield — majority of districts |
| 1 | Large Scale Farming | 281 | Largest harvested area, highest fertilizer demand |
| 2 | High Yield Outlier | 2 | Extremely high yield per hectare (sugarcane-dominant) |
| 3 | Extreme Production Zone | 11 | Abnormally high total production volume |

### Rice Fertilizer Demand Forecast (2016–2017)

| Year | Forecast (ton) | 95% Interval |
|---|---|---|
| 2016 | 4,959,756 | 4,204,868 – 5,644,286 |
| 2017 | 4,933,382 | 4,060,894 – 5,845,986 |

---

## Tech Stack

Python · Pandas · NumPy · Scikit-learn · Prophet · Folium · Geopy · Matplotlib · Seaborn · Google Colab

---

## Repo Structure

```
├── Crop_Production_Analytics.ipynb   # main notebook (CRISP-DM, 6 phases)
├── output_forecast_2016_2018.csv     # forecast results
├── fase2_distribusi.png              # data distribution
├── fase2_tren.png                    # annual trend
├── fase4_elbow_silhouette.png        # K selection
├── fase4_clustering_result.png       # clustering output
├── fase4_classification.png          # confusion matrix + feature importance
├── fase4_ts_decomposition.png        # time series decomposition
├── fase4_ts_forecast.png             # prophet forecast
├── fase4_peta_distribusi.html        # interactive map
└── fase5_evaluation.png              # model evaluation summary
```

---

## How to Run

1. Download the dataset from the Kaggle link above
2. Open `Crop_Production_Analytics.ipynb` in Google Colab
3. Upload the CSV to the Colab session
4. Run all cells from top to bottom
5. Geocoding (~12 min) only runs once — results are cached to `district_coordinates.csv`

---

## Notes

- 2015 data was excluded from the time series model due to incomplete records in the dataset for that year, which caused an unrealistic drop in aggregated values.
- Fertilizer demand is estimated using FAO nitrogen dose guidelines per crop type — not directly observed in the original dataset.
- The `demand_level` target (Low / Medium / High) is defined by tertiles of estimated demand to keep class distribution balanced.
