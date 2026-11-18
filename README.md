# Risk Management Portfolio

Two end-to-end risk modeling projects in Python and Jupyter.

## 1. Credit Risk Scorecard

Credit scoring model on the *Give Me Some Credit* dataset (`Credit Risk Scorecard/data`).

| Step | Folder |
|---|---|
| EDA & data cleaning (missing values, winsorization) | `EDA Cleaning/` |
| WOE binning & Information Value feature selection | `WOE_IV/` |
| Logistic regression & points-based scorecard | `Logistic regression/` |
| Validation, calibration, PSI, gains/lift & model card | `Validation, Calibriation and Model Card/` |

## 2. Insurance Fraud Detection

Fraud classifier on insurance claims data (`insurance_claims.csv`).

1. EDA & class-imbalance analysis
2. Feature engineering
3. SMOTE & XGBoost training, threshold optimization
4. SHAP explainability (summary, dependence, waterfall, decision plots)
5. Streamlit dashboard (`app.py`)

### Run the dashboard

```bash
cd "Insurance fraud detection"
python -m venv fraud_env && source fraud_env/bin/activate
pip install -r requirements.txt
streamlit run app.py
```
