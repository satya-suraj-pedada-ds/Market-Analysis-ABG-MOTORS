# ABG Motors — India Market Entry Propensity Analysis

**Question:** Can a 70,000-row India city sample clear a 12,000-vehicle hurdle if a purchase model is trained on labeled Japan data?

**Call:** Pilot the city. Do not treat the raw India score as a national forecast.

| Scenario | Result | vs 12,000 |
|---|---|---|
| Expected sales (unadjusted logistic) | 60,318 | 5.0x |
| Expected sales (income-aligned logistic) | 40,987 | 3.4x |
| Buyers at p greater than or equal to 0.50 | 67,362 | — |
| Japan actual purchase rate | 23,031 / 40,000 = 57.6% | — |

Planning number: 40,987. Japan and India incomes are not the same unit.

## What this shows a hiring manager

- Classification on a labeled market, scoring on an unlabeled market
- A business hurdle (12,000), not only ROC-AUC
- Domain shift called out, not hidden
- Tableau used to explain the decision
- CRM action: prioritize AGE_SEG 3 and 4

Stack: Python, pandas, scikit-learn, XGBoost, Plotly, Tableau.

## Data

| File | Rows | Role |
|---|---|---|
| data/raw/JPN Data.xlsx | 40,000 | Train and validate (PURCHASE present) |
| data/raw/IN_Data.xlsx | 70,000 | Score only (no purchase label) |
| data/processed/Indian_Scored_Customers.csv | 70,000 | Scored output |

Same AGE_SEG bins in both markets:

- 1: less than 200 days
- 2: 200 to 360 days
- 3: 360 to 500 days
- 4: greater than 500 days

## Method

1. Engineer AGE_SEG from the brief.
2. Fit logistic regression on Japan (decision model).
3. Fit XGBoost on Japan (comparison only).
4. Score India on raw income.
5. Score India again after mapping India income onto the Japan income distribution.
6. Expected sales = sum of probabilities. Compare both scenarios to 12,000.

## Japan holdout

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Logistic (primary) | 68.64% | 74.42% | 69.38% | 71.81% | 0.760 |
| XGBoost (comparison) | 69.79% | 74.78% | 71.73% | 73.22% | 0.781 |

Odds of purchase versus AGE_SEG 1:

- AGE_SEG_4: 10.10x
- AGE_SEG_3: 6.71x
- ANN_INCOME: 1.55x
- AGE_SEG_2: 1.32x
- GENDER_ENC: 1.23x
- CURR_AGE: 0.87x

Japan purchase rate by AGE_SEG: 36.4% to 42.4% to 78.7% to 84.0%.

## Tableau

| Dashboard | Job |
|---|---|
| 1 Decision | Does the sample clear 12,000? |
| 2 Drivers | Is the Japan model usable? |
| 3 CRM | Who should sales call? |
| 4 Japan baseline | What does the labeled market do? |

Open tableau/ABG Motors – India Market Entry Decision.twbx.

Dashboard images:

- dashboards/Dashboard-1.jpg
- dashboards/Dashboard-2.jpg
- dashboards/Dashboard-3.jpg
- dashboards/Dashboard-4.jpg

## How to run
pip install -r requirements.txt

Run notebook/ABG-Motors-India-Market-Entry-Propensity-Analysis.ipynb with JPN Data.xlsx and IN_Data.xlsx in the notebook working folder. 

## Repo
├── README.md
├── requirements.txt
├── data/
│   ├── raw/
│   │   ├── JPN Data.xlsx
│   │   └── IN_Data.xlsx
│   └── processed/
│       ├── Indian_Scored_Customers.csv
│       ├── Logistic_Coefficients.csv
│       └── Model_Performance_Comparison.csv
├── notebook/
│   └── ABG-Motors-India-Market-Entry-Propensity-Analysis.ipynb
├── tableau/
│   └── ABG Motors – India Market Entry Decision.twbx
└── dashboards/
    ├── Dashboard-1.jpg
    ├── Dashboard-2.jpg
    ├── Dashboard-3.jpg
    └── Dashboard-4.jpg
```

## What this is not

- Not a national India forecast
- Not a causal lift test
- Not validated against actual India purchases (there is no India label)

Next step: city pilot, rank AGE_SEG 3 and 4, A/B the CRM offer, then scale only if conversion holds.



