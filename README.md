# Structural Changes in Household Consumption During Economic Stress

Data Mining course project, Faculty of Computer Science and Engineering (FINKI), Ss. Cyril and Methodius University, Skopje.

## Overview

This project analyzes monthly U.S. household consumption data to see whether
the composition of spending changes during economic crises, and whether
changepoint detection methods can identify these shifts automatically.

The analysis uses aggregated monthly series (one row = one month) from the
U.S. Bureau of Economic Analysis (BEA) and the Federal Reserve Economic Data
(FRED) database, covering January 1990 onward. Stress periods examined
include the 2008–2009 financial crisis, the 2020 COVID shock, and the
post-2021 inflation period.

## Research questions

1. **Does the structure of consumption change during economic crises?**
   Comparison of normal and crisis periods in terms of discretionary vs.
   necessity spending and the goods/services split, using EDA, period
   comparisons, confidence intervals, hypothesis tests, and regression
   with crisis indicators.

2. **Can changepoint methods automatically detect structural breaks in
   consumption indicators?**
   Changepoint detection (e.g., PELT, binary segmentation) applied to one or
   two key series, with detected breaks compared against known crisis dates.

3. **Optional: are there distinct consumption regimes?**
   PCA and clustering of months based on spending shares and macroeconomic
   indicators.

## Data sources

| Source | Table / Series | Content |
|---|---|---|
| BEA, NIPA Underlying Detail | Table 2.4.5U | PCE by type of product, nominal (billions of $, SAAR) |
| BEA, NIPA Underlying Detail | Table 2.4.6U | PCE by type of product, real (chained 2017 $) |
| BEA, NIPA Underlying Detail | Table 2.4.4U | PCE price indexes (2017 = 100) |
| FRED | `UNRATE` | Unemployment rate (%) |
| FRED | `CPIAUCSL` | Consumer Price Index |
| FRED | `PCEPI` | PCE price index |
| FRED | `PSAVERT` | Personal saving rate (%) |
| FRED | `UMCSENT` | University of Michigan consumer sentiment |
| FRED | `USREC` | NBER recession indicator (0/1) |

All series are monthly and seasonally adjusted where applicable.

**Download date:** TODO
BEA revises historical data regularly, so results are tied to the
raw files stored in `data/raw/`.

A full description of every variable, its source and its transformation
is in [`docs/data_dictionary.csv`](docs/data_dictionary.csv).

## Key methodological choices

- **Spending shares are computed from nominal series.** Real (chained-dollar)
  components are not additive, so ratios of real series are not valid shares.
- **Growth rates are computed from real series**, so they reflect changes in
  quantity rather than price.
- Growth rates are log differences: month-over-month and year-over-year.
- Crisis periods (current definitions, see notebook 02 for justification):
  - Financial crisis: 2007-12 to 2009-06 (NBER recession dates)
  - COVID shock: TODO
  - Inflation period: TODO

## Project structure

```
pce-consumption-stress/
├── data/
│   ├── raw/            # Original downloads from BEA and FRED (never modified)
│   └── processed/      # Cleaned and merged datasets produced by notebooks
├── notebooks/          # Analysis notebooks, run in numerical order
├── src/                # Reusable Python functions
├── reports/figures/    # Saved plots
├── docs/               # Data dictionary and documentation
├── requirements.txt
└── README.md
```

## Notebooks

| Notebook | Purpose |
|---|---|
| `01_data_collection.ipynb` | Download raw data from BEA and FRED |
| `02_preprocessing.ipynb` | Align dates, merge sources, derive shares, growth rates and crisis indicators |
| `03_eda.ipynb` | Exploratory analysis and visualizations |
| `04_rq1_crisis_comparison.ipynb` | Period comparisons, tests, regression with crisis indicators |
| `05_rq2_changepoint.ipynb` | Changepoint detection and comparison with known crises |
| `06_clustering.ipynb` | PCA and clustering of consumption regimes (optional) |

## Setup

Requires Python 3.13.

```bash
git clone <repo-url>
cd pce-consumption-stress
python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # macOS/Linux
pip install -r requirements.txt
```

Create a `.env` file in the project root with a free FRED API key
(from https://fredaccount.stlouisfed.org):

```
FRED_API_KEY=your_key_here
```

Then run the notebooks in order, starting from `01_data_collection.ipynb`.

## Status

- [x] Project setup
- [ ] Data collection
- [ ] Preprocessing and feature construction
- [ ] Exploratory analysis
- [ ] RQ1: crisis period comparison
- [ ] RQ2: changepoint detection
- [ ] Optional: clustering of consumption regimes
- [ ] Final report