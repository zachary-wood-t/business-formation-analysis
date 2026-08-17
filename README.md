# U.S. Business Formation Analysis - Sector Focus

An analysis of the U.S. Census Bureau's Monthly Business Formation Statistics,
looking specifically at which sectors produce the most business applications, when those
applications get filed, and which of them actually turn into real employers.

**Source:** [U.S. Census Bureau, Monthly Business Formation Statistics (2004-2026)](https://www.census.gov/econ/bfs/current/index.html)

## Business questions

1. Which sectors produce the most new business applications, and has the overall sector mix changed over time?
2. Is there any seasonality to when applications are filed?
3. Which sectors have the highest rate of converting applications into real businesses?
4. Can we predict conversion rate for a given sector and month from sector, month, and application composition? **(Machine learning question)**

## Key definitions

- **Application:** an EIN filing with the IRS
- **Formation:** a business prospect that becomes a real employer, signified by having a first instance of payroll
- **Conversion rate:** the share of business prospects (applications) that become employers (formations)

## Repository contents

```
business-formation-analysis/
├── README.md
├── business_formation_analysis.ipynb
└── data/
    ├── bfs_monthly.csv
    └── bfs_monthly_data_dictionary.pdf
```

- `business_formation_analysis.ipynb` - full analysis notebook with all cells run and notated with findings
- `data/bfs_monthly.csv` - Business Formation Statistics source data with monthly and aggregated records, from 2004 to 2026
- `data/bfs_monthly_data_dictionary.pdf` - data dictionary provided by the Census Bureau, defining columns and values

## Approach

The notebook works through assessment, cleaning, exploration, and modeling. 
Some key considerations:

- **The month columns arrive as text.** Census uses `D` where an estimate was withheld
  to avoid disclosing an individual establishment, and `S` where it did not meet
  publication quality standards. These codes are recorded in month columns in some
  instances, and these records were ultimately removed.
- **Outcome analysis stops at 2022.** `BF_BF4Q` measures an application actually
  becoming a business. From 2023 onward, this is only recorded as projections and spliced estimates.
  We cut off at 2022 so every formation in scope is a measured data point rather than a modeled one.
- **Sector work is national only.** Sector and state are not recorded in a way that
  lets them be cross-evaluated, so state level data is deprioritized throughout.

The model is a linear regression on sector, month, planned-wages share, and
corporation share, predicting conversion rate.

## Environment

Python with pandas, numpy, scikit-learn, matplotlib, and seaborn packages imported. The notebook is
published with its output cells populated and is meant to be read rather than re-run.

## Limitations

- State cannot be used reliably, since sector-level data is only recorded at the national level.
- True formation measurements end in 2022, so the most recent years of applications are not evaluated against outcomes.
- Historical context is largely ignored, despite changing market conditions across the timeline of the analysis.
