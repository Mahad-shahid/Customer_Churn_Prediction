 🤖 Customer Churn Prediction — Machine Learning

A machine learning project that predicts customer churn using a time-based
train/predict split on 500,000+ real retail transactions. Built on top of
the UCI Online Retail Dataset with a Random Forest classifier and iterative,
error-driven feature engineering.

   Project Overview
Rather than defining churn using arbitrary segment labels, this project uses
a proper temporal split to create ground-truth churn labels:

- Observation Window (Dec 2010 – Sep 2011): All behavioral features are
  built exclusively from this period
- Prediction Window (Oct 2011 – Dec 2011): Used only to determine whether
  each customer actually returned — no data leakage

This makes the model genuinely predictive: given 9 months of purchase
behavior, predict whether a customer will return in the next 3 months.

   Key Results
- Final Accuracy: 68.10% | ROC-AUC: 0.7343
- 77% of actual churners correctly identified (recall)
- £1,170,268 in combined revenue flagged across HIGH and MEDIUM risk tiers
- Improved from 62.27% baseline through four disciplined iterations

   Performance Progression

| Stage | Accuracy | ROC-AUC |
|---|---|---|
| Baseline (5 RFM features) | 62.27% | 0.6855 |
| + Behavioral features | 66.00% | 0.7200 |
| + Repeat flag + Velocity | 66.30% | 0.7274 |
| + Error-driven features | 68.10% | 0.7343 |

   Key Findings
- Monetary value (0.132), Frequency (0.115), UniqueProducts (0.114),
  and UniqueMonths (0.105) are the four strongest churn predictors
- Recency ranked only 6th — contrary to standard RFM assumptions,
  engagement depth predicts retention better than recency alone
- Error analysis revealed two distinct failure modes: irregular/seasonal
  buyers who look like churners but return, and sudden stoppers who look
  loyal but abruptly leave
- Features like `RecencyRatio` and `PurchaseConsistency` were engineered
  directly from error analysis findings — not added arbitrarily

   Project Structure

- Cell 1 — Imports & Setup
- Cell 2 — Data Loading & Cleaning
- Cell 3 — Phase 1: Feature Engineering & Churn Labeling
- Cell 4 — Phase 2: Baseline Model (62.27%)
- Cell 5 — Phase 2B: Feature Enrichment (66.00%)
- Cell 6 — Error Analysis: Diagnosing Model Failures
- Cell 7 — Phase 2C: Error-Driven Features + Final Model (68.10%)
- Cell 8 — Phase 3: Model Evaluation & Visualizations
- Cell 9 — Phase 4: Business Output & Risk Tier Scoring
- Cell 10 — Final Summary & Business Recommendations

   Features Used (Final Model — 12 features)

| Feature | Description | Type |
|---|---|---|
| Recency | Days since last purchase | RFM Core |
| Frequency | Total number of orders | RFM Core |
| Monetary | Total spend | RFM Core |
| AvgOrderValue | Spend per order | Derived |
| PurchaseSpread | Days between first and last purchase | Derived |
| UniqueProducts | Number of distinct products bought | Behavioral |
| UniqueMonths | Number of distinct months active | Behavioral |
| AvgDaysBetween | Natural purchase rhythm | Behavioral |
| SpendVelocity | Late spend minus early spend | Trend |
| RecencyRatio | Recency relative to own purchase rhythm | Error-driven |
| PurchaseConsistency | Std deviation of inter-purchase gaps | Error-driven |
| RecentFrequencyRatio | Recent vs early purchase frequency | Error-driven |

Business Output
Every customer receives a churn probability score and is assigned to a
risk tier:

| Risk Tier | Churn Probability | Recommended Action |
|---|---|---|
| HIGH RISK | ≥ 70% | Personal outreach, priority retention |
| MEDIUM RISK | 50–70% | Automated win-back campaigns |
| LOW RISK | 30–50% | Monitoring, light engagement |
| SAFE | < 30% | Standard communications |

  Known Limitations
- Prediction window (Oct–Dec) overlaps with holiday season which may
  inflate retention rates due to seasonal purchasing behavior
- Reaching above 70% accuracy likely requires demographic or behavioral
  data beyond transaction history alone
- 68% accuracy on a genuinely noisy, real-world churn problem with pure
  transaction data is a realistic and honest result

  Tech Stack
- Python
- Pandas
- NumPy
- Scikit-learn (RandomForestClassifier, RandomizedSearchCV)
- Matplotlib
- Seaborn
- Google Colab

  How to Run
1. Install dependencies:
   pip install pandas numpy scikit-learn matplotlib seaborn openpyxl
2. Download the dataset from:
   https://archive.uci.edu/dataset/352/online+retail
3. Upload `Online Retail.xlsx` to your Colab session
4. Run all cells in order

  Data Source
UCI Machine Learning Repository — Online Retail Dataset
541,909 transactions from a UK-based online retailer (2010–2011)
