# Singapore Bunker Fuel Market Analysis

An end-to-end data analysis of Singapore's marine bunker fuel market (1995-2025), investigating the impact of IMO 2020 sulphur regulations on fuel mix, market concentration, and future demand trends.

## Background

In January 2020, the International Maritime Organization (IMO) enforced a global sulphur cap of 0.50% (down from 3.50%) on marine fuels. This regulation fundamentally reshaped the bunker fuel market, forcing shipping companies to either:

1. Switch to compliant low-sulphur fuels
2. Install exhaust gas scrubbers to continue using high-sulphur fuel
3. Adopt alternative fuels like LNG or methanol

This project analyses Singapore's bunker sales data — the world's largest bunkering port — to quantify these shifts and forecast future trends.

## Key Findings

- **MFO to LSFO was the dominant shift** — Marine Fuel Oil (MFO) dropped from ~80% to ~26% market share within two years, replaced almost entirely by Low Sulphur Fuel Oil (LSFO)
- **The transition was abrupt** — Monthly data shows the MFO/LSFO crossover occurred almost entirely within Dec 2019 - Jan 2020
- **Total market volume was unaffected** — The regulation reshuffled the fuel mix without shrinking overall demand (~50,000+ kt annually)
- **MFO is recovering** — As more vessels install scrubbers, MFO is regaining share (+2,329 kt/yr post-2020)
- **LNG is emerging** — Growing at ~149 kt/yr but still under 1% of total volume
- **Oil prices have limited influence** — Regulatory shifts drive volumes more than crude price movements

## Project Structure

```
├── data/
│   ├── raw/                     # Source CSVs (annual & monthly bunker sales)
│   ├── cleaned/                 # Processed datasets with fuel categories & compliance tiers
├── notebooks/
│   ├── Data_Collection.ipynb    # Data loading, cleaning, and categorisation
│   ├── market_overview.ipynb    # Trends, growth rates, fuel mix, CAGR, IMO 2020 deep-dive
│   ├── Risk_analysis.ipynb      # Oil price sensitivity, HHI, substitution dynamics, FX exposure
│   └── Forecasting.ipynb        # Linear trend forecasts, oil price scenarios, seasonal decomposition
└── .env                         # API keys (FRED_API_KEY)
```

## Notebooks

### 1. Data Collection (`Data_Collection.ipynb`)
- Loads raw bunker sales data (16 fuel types, 1995-2025)
- Consolidates into 8 simplified categories: MFO, LSFO, ULSFO, MGO, LSMGO, MDO, LNG, Alternative
- Maps fuels to compliance tiers (Non-Compliant, IMO 2020 Compliant, ECA Compliant, Alternative)
- Fetches Brent and WTI crude oil price data from FRED API

### 2. Market Overview (`market_overview.ipynb`)
- Total annual bunker sales trends and year-over-year growth
- Stacked area charts for fuel mix (absolute and market share)
- Monthly zoom on the 2018-2022 IMO 2020 transition period
- Pre vs post-IMO 2020 CAGR comparison by fuel type
- 2019 vs 2021 fuel mix deep-dive with substitution pair analysis

### 3. Risk Analysis (`Risk_analysis.ipynb`)
- Oil price sensitivity: Pearson correlations between Brent crude and fuel volumes
- Rolling 24-month correlations to track evolving price relationships
- Herfindahl-Hirschman Index (HHI) for market concentration risk
- MFO vs LSFO substitution dynamics
- USD/SGD exchange rate exposure analysis
- Consolidated risk summary table

### 4. Forecasting (`Forecasting.ipynb`)
- Linear trend forecasts for top 5 fuels (2026-2030) with 95% prediction intervals
- Oil price as external regressor (trend + Brent model with LOO cross-validation)
- Scenario analysis: base, high oil (+20%), low oil (-20%)
- Seasonal decomposition of monthly demand patterns
- Business recommendations per fuel category

## Data Sources

- **Bunker sales**: Singapore Maritime and Port Authority (MPA) — annual and monthly breakdowns by fuel type
- **Crude oil prices**: FRED API — Brent (DCOILBRENTEU) and WTI (DCOILWTICO) daily series
- **Exchange rates**: FRED API — USD/SGD (DEXSIUS)

## Setup

### Prerequisites
- Python 3.9+
- A [FRED API key](https://fred.stlouisfed.org/docs/api/api_key.html)

### Installation

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn statsmodels fredapi python-dotenv
```

Create a `.env` file in the project root:
```
FRED_API_KEY=your_api_key_here
```

### Usage

Run the notebooks in order (1-4), or export all Tableau data at once:

```bash
python scripts/export_tableau_data.py
```

## Tools & Technologies

- **Python**: pandas, numpy, matplotlib, seaborn, scipy, scikit-learn, statsmodels
- **FRED API**: Economic data retrieval via `fredapi`
- **Tableau**: Interactive dashboards built from exported CSVs
