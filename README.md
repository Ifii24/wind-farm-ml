# Machine Learning for Wind Farm Output Forecasting

**Advanced Business Analytics — DTU (Technical University of Denmark)**
**Course 42578 | April 2023**

A four-part data science project exploring how machine learning can help energy companies schedule operations around wind energy production and reducing reliance on fossil fuels and minimising exposure to CO₂ taxation.

---

## The problem

Energy-intensive industries (data centres, manufacturing, aluminium smelting) consume large amounts of electricity around the clock. With Denmark introducing a corporate carbon tax of up to **1,125 DKK/tCO₂ (≈ €150)** from 2025, companies have a strong financial incentive to shift high-consumption operations to windows when wind farms are producing clean electricity.

To do that, they need accurate short-term forecasts of wind farm output. This project builds  a pipeline that goes from raw weather data to predicted power output for specific wind farm locations across Europe.

---

## Project structure

```
wind-farm-ml/
├── notebooks/
│   ├── part_1_weather_prediction.ipynb          # Wind speed forecasting
│   ├── part_2_capacity_factor_prediction.ipynb  # Power output modelling
│   ├── part_3_potential_location_filtering.ipynb # New site identification
│   └── part_4_case_studies.ipynb               # End-to-end validation
└── docs/
    ├── executive_summary.pdf
    └── highlights_and_limitations.pdf
```

---

## The four parts

### Part 1 — Wind Speed Forecasting
**Goal:** Predict wind speed 4 hours ahead at four wind farm locations (Denmark, Greece, Spain, Bulgaria).

Weather data is collected from open meteorological APIs and the Global Wind Power Tracker. The analysis includes stationarity testing (Augmented Dickey-Fuller), Granger causality tests to identify which weather variables actually drive wind speed, and time-series forecasting using **Facebook Prophet** with multivariate regressors. The 4-hour horizon was chosen as the minimum actionable window for industrial load scheduling.

Also includes a web scraper (Selenium + BeautifulSoup) to collect live wind farm production data from windpowerenergy.net for benchmarking.

### Part 2 — Capacity Factor Prediction
**Goal:** Given predicted wind speed, predict the capacity factor (actual output / maximum possible output) of a wind farm.

Uses historical capacity factor data from a large European dataset (84 million rows across thousands of onshore wind sites). Multiple regression models are trained and compared. The best-performing model is then chained with Part 1's wind speed forecasts to produce an end-to-end power output prediction.

### Part 3 — Identifying New Wind Farm Locations
**Goal:** Filter suitable locations across Europe for potential new wind farm construction.

Combines land use data from the EU's GISCO dataset (Eurostat), electricity grid infrastructure data, and topological constraints. GeoPandas is used for spatial analysis and filtering — areas like forests, urban zones, water bodies, and protected land are excluded. The output is a ranked list of candidate sites with favourable wind conditions and viable grid access.

### Part 4 — Case Studies
**Goal:** Validate the full pipeline on real-world examples.

Two case studies:
1. **New location selection** — applies the capacity factor models to the filtered candidate sites from Part 3, selecting the most promising locations based on predicted average annual output.
2. **Danish wind farm validation** — applies the full weather → capacity factor pipeline to a real Danish wind farm and benchmarks predictions against actual production data scraped from windpowerenergy.net.

---

## Key techniques

| Topic | Methods used |
|-------|-------------|
| Time series forecasting | Facebook Prophet, multivariate regressors |
| Stationarity testing | Augmented Dickey-Fuller test |
| Causality analysis | Granger causality matrix |
| Regression modelling | Multiple models compared (Part 2) |
| Spatial analysis | GeoPandas, Eurostat GISCO land use data |
| Data collection | REST APIs, Selenium web scraping |
| Data processing | pandas, xarray (NetCDF climate datasets) |
| Visualisation | matplotlib, seaborn, folium (interactive maps) |

---

## Data sources

- **Global Wind Power Tracker** — [globalenergymonitor.org](https://globalenergymonitor.org/projects/global-wind-power-tracker/) — global database of wind facilities
- **Open-Meteo / ERA5** — historical and forecast weather data
- **EU GISCO** — [ec.europa.eu/eurostat](https://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/land-cover) — European land use and coverage data
- **windpowerenergy.net** — real-time wind farm production (web scraped)
- **Renewables Ninja** — capacity factor time series data

---

## How to run

The notebooks run sequentially — Part 1 outputs feed into Part 2, Part 2 feeds into Part 4.

```bash
pip install pandas numpy matplotlib seaborn scikit-learn prophet xarray geopandas folium selenium requests
jupyter notebook
```

Open notebooks in order:
1. `notebooks/part_1_weather_prediction.ipynb`
2. `notebooks/part_2_capacity_factor_prediction.ipynb`
3. `notebooks/part_3_potential_location_filtering.ipynb`
4. `notebooks/part_4_case_studies.ipynb`

> **Note:** Some data files (NetCDF climate datasets, shapefiles) are large and not included in this repository. See the data collection sections at the start of each notebook for instructions on downloading them.

---

## Results summary

The full pipeline successfully predicts wind farm power output 4 hours ahead with reasonable accuracy across four European locations. The capacity factor models outperform baseline persistence forecasts. The location filtering methodology identified a ranked list of candidate sites in Italy and Portugal with above-average wind resources and viable grid connectivity.

See `docs/executive_summary.pdf` for the full results and `docs/highlights_and_limitations.pdf` for a detailed discussion of model performance and known limitations.

---

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
prophet
xarray
netcdf4
geopandas
folium
selenium
webdriver-manager
requests
openpyxl
warnings
```
