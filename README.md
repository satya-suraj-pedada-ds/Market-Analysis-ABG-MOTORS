# ABG Motors – India Market Entry Analysis

Predicting new-vehicle purchase propensity to decide whether ABG Motors should enter India.

Decision: ENTER  
Sample expected sales: ~60,318  
Target: 10,000 vehicles  
Multiple of target: ~6.0x

## Business Problem
ABG Motors (fictional Japan-based automaker) wants to know if the Indian market can support at least 10,000 vehicle sales. The company provided Japanese purchase history and an Indian customer sample.

## Approach
- Trained a Logistic Regression model on 40,000 Japanese customers as the primary interpretable model
- Benchmarked against XGBoost
- Applied company-mandated car-age segments:
  - 1: <200 days
  - 2: 200–360 days
  - 3: 360–500 days
  - 4: >500 days
- Scored 70,000 Indian customers
- Built 3 Tableau dashboards for executive decision, model drivers, and CRM targeting

## Key Results
| Metric | Value |
|---|---|
| Logistic ROC-AUC | 0.76 |
| XGBoost ROC-AUC | 0.78 |
| Strongest driver | AGE_SEG_4 odds ratio ≈ 10.10x |
| Expected sales (sum of probabilities) | 60,318 |
| Predicted buyers (p ≥ 0.5) | 67,362 |
| Mean predicted probability | ~0.86–0.90 |

## Business Insight
Customers whose vehicle is more than 500 days since last maintenance have about 10 times higher odds of buying a new car than customers with <200 days, holding other factors constant.

## Recommendation
Enter the Indian market based on the sample signal, but run a local pilot before national rollout.

## Risks
1. Income is in different currencies/scales (Yen vs INR)
2. Indian sample appears concentrated, not a full national census
3. Model is correlational, not causal — needs A/B testing in CRM

## Tableau Dashboards
1. Executive Decision – expected sales vs 10,000 target
2. Model Performance & Drivers – metrics + odds ratios
3. Indian Opportunity & CRM – segment targeting by probability threshold

## Repository Structure
```text
abg-motors-india-market-entry/
├── README.md
├── requirements.txt
├── notebooks/
│   └── ABG-Motors-India-Market-Entry-Propensity-Analysis.ipynb
├── data/
│   ├── raw/
│   │   ├── JPN_Data.xlsx
│   │   └── IN_Data.xlsx
│   └── processed/
│       ├── Indian_Scored_Customers.csv
│       ├── Logistic_Coefficients.csv
│       └── Model_Performance_Comparison.csv
├── tableau/
│   └── ABG Motors – India Market Entry Decision.twb
└── images/
    ├── dashboard1_executive.png
    ├── dashboard2_model_drivers.png
    └── dashboard3_crm.png



## How to Run
```bash
cd C:\Users\DELL
pip install -r requirements.txt
jupyter notebook ABG-Motors-India-Market-Entry-Propensity-Analysis.ipynb
