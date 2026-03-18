# Housing Affordability Analysis

## Brief Description
This project uses American Community Survey (ACS) Public Use Microdata Sample (PUMS) housing data to examine housing affordability across U.S. Public Use Microdata Areas (PUMAs). The notebook constructs household-level housing burden measures, maps affordability and severe burden nationally, compares renters and owners, groups similar PUMAs with K-means clustering, and estimates regression models to identify household characteristics associated with higher burden.

## Notebook
- Main notebook: Housing_Affordability.ipynb
  
## Data Sources
- ACS PUMS 2023 1-year housing files:
  - National housing ZIP: [`csv_hus.zip`](https://www2.census.gov/programs-surveys/acs/data/pums/2023/1-Year/csv_hus.zip)
  - Census directory: [2023 ACS PUMS 1-Year Files](https://www2.census.gov/programs-surveys/acs/data/pums/2023/1-Year/)
  - After extraction, the notebook uses:
    - `psam_husa.csv`
    - `psam_husb.csv`
- Census TIGER/Line 2023 PUMA shapefiles:
  - Census directory: [2023 TIGER/Line PUMA shapefiles](https://www2.census.gov/geo/tiger/TIGER2023/PUMA/)
  - Download the state ZIP files named like `tl_2023_06_puma20.zip` and extract them

## Quick Download Option
- Script: [scripts/download_housing_data.sh]
- Example usage:
  - `bash scripts/download_housing_data.sh`
  - or `bash scripts/download_housing_data.sh /path/to/data_dir`
- The script downloads:
  - the ACS 2023 1-year housing ZIP
  - 2023 TIGER/Line PUMA shapefiles for all 50 states plus DC
- After running it, update the notebook paths if your data lives somewhere other than the default locations used in the notebook.

## What the Notebook Does
1. Loads and combines ACS housing microdata.
2. Filters to households with positive income.
3. Creates monthly housing cost, annual housing cost, and burden-ratio measures.
4. Aggregates weighted affordability measures to the PUMA level using ACS household weights.
5. Builds national maps of average burden and severe burden.
6. Compares severe burden rates for renters and owners.
7. Uses K-means clustering to group PUMAs with similar affordability profiles.
8. Fits regression models to estimate how tenure, household size, building type, and region are associated with housing burden.

## Key Variables
- `burden_ratio`: annual housing cost divided by annual household income
- `avg_burden`: weighted average burden ratio within a PUMA
- `severe_burden_rate`: share of households spending more than 50% of income on housing
- `TEN`: tenure category (used to compare owners and renters)
- `WGTP`: ACS household weight
- `STATE` + `PUMA`: combined geographic key for matching ACS records to Census PUMA boundaries

## Methods Used
- Weighted descriptive statistics
- Choropleth mapping with GeoPandas
- K-means clustering
- Linear regression for capped burden ratio
- Logistic regression for high-burden status (`burden_ratio > 0.30`)

## Main Takeaways
- Housing affordability varies meaningfully across PUMAs.
- Severe burden rates highlight places where affordability stress is especially acute.
- Renters generally face higher severe burden rates than owners.
- K-means clustering shows that PUMAs can share similar affordability profiles even when they are far apart geographically.
- Regression results suggest that tenure is the strongest observed correlate of housing burden in this specification, though much variation remains unexplained.

## Requirements
Common Python packages used in the notebook include:
- `pandas`
- `geopandas`
- `matplotlib`
- `scikit-learn`
- `mapclassify`

## Notes
- PUMA codes are not unique nationally, so the notebook always uses both `STATE` and `PUMA` together.
- Quantile map classification is used to improve visual contrast across PUMAs.
- The regression models are descriptive and should not be interpreted causally.
