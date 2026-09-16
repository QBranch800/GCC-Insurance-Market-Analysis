# GCC Insurance Market Analysis

## Overview
An exploratory data analysis (EDA) evaluating insurance market attractiveness 
across the six GCC countries (UAE, Saudi Arabia, Qatar, Kuwait, Bahrain, Oman), 
with a deeper trend analysis of the UAE insurance market by product line.


## Setup
This project uses a Python virtual environment. 
To reproduce it:
- python3 -m venv venv
- source venv/bin/activate
- pip install -r requirements.txt


## Business Task
"Which GCC country should an insurer prioritize for expansion of insurance 
business, and having identified the UAE as a focus market, what product-line 
trends over the past several years should shape their UAE entry strategy?"

## Key Findings

- **UAE is the recommended priority market** — it leads the GCC on insurance 
  penetration (3.58%), density ($1,799 per capita), and product diversification 
  (17.1% life insurance share), sitting on a comparatively stable wealth 
  trajectory rather than a volatile peak.
**Health and Motor insurance should lead UAE entry strategy** — Health insurance is the largest segment (USD 9,918M, 48.7% of total 2025 GWP); Motor is the fastest-growing line by both growth rate (+113%, 2021–2025) and CAGR (20.8%).
- **Group life products outperform individual/retail life** — Individual 
  Life, Group Credit Life, and Annuities all declined over 2021–2025, while 
  Group Life grew steadily (+14.8% CAGR).
- **Two secondary markets are worth monitoring, not prioritizing**: Qatar 
  (high wealth, low penetration — likely structural) and Oman (fastest 
  population growth in the GCC, but currently weakest on wealth and 
  penetration).

Full analysis and supporting visualizations: [`04_analysis.ipynb`](04_analysis.ipynb). 

Presentation deck: [`GCC_Insurance_Market_Analysis.pptx`](GCC_Insurance_Market_Analysis.pptx).

## Metrics
The following metrics, drawn from standard insurance industry terminology, 
are used throughout this analysis:

- **Gross Written Premium (GWP):** The total premium value written by 
  insurers in a given year, before deducting reinsurance costs. Used as 
  the primary measure of market size.
- **Insurance Penetration:** GWP expressed as a percentage of a country's 
  GDP. Indicates how large the insurance sector is relative to the overall 
  economy — a higher percentage suggests a more developed insurance market.
- **Insurance Density:** GWP divided by population (premium per capita). 
  Indicates how much, on average, each person in a country spends on 
  insurance annually — a useful complement to penetration, since a small 
  wealthy population can have high density but modest penetration, or 
  vice versa.
- **CAGR (Compound Annual Growth Rate):** The annualized growth rate of a 
  metric (e.g., GWP) over a multi-year period, smoothing out year-to-year 
  volatility to show the underlying growth trend.
- **Life vs. Non-life Split:** The share of total GWP attributable to life 
  insurance (e.g., whole life, term life) versus non-life/general insurance 
  (e.g., motor, health, property). Used in the UAE deep-dive to track how 
  the product mix has shifted over time.
- **GCC Market Share:** A country's GWP as a percentage of the combined 
  GWP of all six GCC countries. Used to rank market size within the region.

## Data Sources
| Source | Coverage | Method |
|---|---|---|
| Alpen Capital "GCC Insurance Industry" report (May 20, 2026) | GWP, penetration, density, life/non-life split — 6 GCC countries, 2025 (estimated) | Extracted directly from forecast charts, pages 39–45 (Exhibits 41–54) |
| World Bank Open Data (World Development Indicators) | GDP, population, GDP per capita — 6 GCC countries, 2010–2025 | Pulled via API |
| Central Bank of the UAE, Annual Statistical Reports (2022–2025 editions) | UAE gross written premium by line of business — 2021–2025 | Manually extracted from Table 2A in each edition, cross-validated across overlapping years |

## Data Quality
Both raw datasets were systematically checked for shape, missing values, 
duplicates, and logical consistency after collection.

- Saudi Arabia and Bahrain's life/non-life GWP shares each sum to slightly 
  under 100% (Saudi: 99.5%, Bahrain: 97.5%) — a small, recurring rounding 
  artifact in the source report's chart values, not a data entry error. 
  Retained as reported.
- World Bank data uses "United Arab Emirates" while the insurance dataset 
  uses "UAE"; standardized to "UAE" in cleaning.
- World Bank `year` column was stored as text, not integer; converted in 
  cleaning.
- UAE's 2025 GDP was not yet published by World Bank at time of retrieval; 
  resolved by using each country's most recent available year (UAE: 2024, 
  all others: 2025), tracked transparently via a `macro_data_year` column.
- UAE line-of-business figures were cross-validated against a second 
  independent source (Alpen Capital), landing within 0.6% of each other.

Full quality-check methodology and findings are documented inline in 
`01_data_collection.ipynb`.

## Project Structure
- `01_data_collection.ipynb` — pulling, saving, and quality-checking raw 
  GCC insurance and World Bank macro data
- `02_cleaning.ipynb` — standardizing country names, fixing data types, 
  and merging both datasets into an analysis-ready table
- `03_uae_data_collection_cleaning.ipynb` — sourcing and cleaning UAE 
  historical line-of-business data from CBUAE annual reports
- `04_analysis.ipynb` — exploratory analysis, 11 visualizations, and key 
  findings across both the cross-GCC comparison and UAE deep dive
- `data/raw/` — unprocessed source files
- `data/cleaned/` — cleaned, analysis-ready datasets
- `exports/` — chart images exported for the presentation deck
- `GCC_Insurance_Market_Analysis.pptx` — presentation deck summarizing 
  the analysis for a stakeholder audience

## Further Exploration
- A comparable line-of-business breakdown for Saudi Arabia, to compare 
  entry-line strategy across both priority markets
- Regulatory calendar research (e.g., mandatory health insurance rollout 
  dates by emirate) to confirm the causal link suggested for UAE's 
  health/motor growth
- A dedicated investigation into Qatar's insurance market structure, to 
  test whether the wealth/penetration gap is genuinely inaccessible or 
  contains an addressable segment (e.g., expat-specific products)
- Tracking Oman's insurance penetration alongside its population growth 
  over the next several years, to assess whether demand is beginning to 
  catch up to its demographic expansion

