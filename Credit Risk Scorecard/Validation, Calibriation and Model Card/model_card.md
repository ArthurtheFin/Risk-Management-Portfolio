# Model Card — Credit Risk Scorecard

**Version:** 1.0  
**Date:** 2026-05-08  
**Author:** [Your Name]  
**Status:** Development / Portfolio Project

---

## 1. Model Overview

| Field | Detail |
|---|---|
| Model type | Logistic Regression with WOE encoding |
| Target variable | `target` — P(serious delinquency within 2 years) |
| Training dataset | Give Me Some Credit (Kaggle, 2011) |
| Training size | 105,000 records |
| Test size | 45,000 records |
| Score range | 300 – 850 (PDO=20, Base=600, Odds₀=1/19) |

---

## 2. Features Used (9 variables)

| Variable | Description | IV Strength |
|---|---|---|
| `revolving_util` | Revolving credit utilization rate | Strong |
| `late_90_days` | Number of times 90+ days past due | Strong |
| `past_due_30_59` | Number of times 30-59 days past due | Strong |
| `past_due_60_89` | Number of times 60-89 days past due | Medium |
| `age` | Age of borrower | Medium |
| `debt_ratio` | Monthly debt payments / gross income | Medium |
| `monthly_income` | Monthly gross income | Medium |
| `real_estate_loans` | Number of real estate loans | Weak |
| `dependents` | Number of dependents | Weak |

**Excluded:** `open_credit_lines` (sign reversal — non-monotone relationship)

---

## 3. Performance Metrics

| Metric | Train | Test | Benchmark |
|---|---|---|---|
| AUC | 0.8528 | 0.8538 | > 0.75 ✓ |
| Gini | — | 70.76% | > 50% ✓ |
| KS | — | 55.38% | > 40% ✓ |
| Brier Score | — | 0.1438 | < 0.06 ✓ |
| PSI (train vs test) | — | 0.0003 | < 0.10 ✓ |

---

## 4. Known Limitations & Risks

1. **Data vintage:** Dataset từ năm 2009 (US financial crisis) — có thể không đại diện cho điều kiện thị trường hiện tại
2. **Geography:** Dữ liệu từ thị trường Mỹ — cần validation trước khi áp dụng cho Việt Nam
3. **Missing income:** `monthly_income` missing 19.8% → imputed bằng median → có thể bias predictions cho nhóm này
4. **Open credit lines:** Bị loại do sign reversal — có thể capture thêm predictive power nếu dùng non-linear model
5. **Class imbalance:** Bad rate chỉ 6.7% — cần monitor nếu bad rate thực tế thay đổi đáng kể

---

## 5. Monitoring Plan

| Metric | Frequency | Action threshold | Action |
|---|---|---|---|
| PSI | Monthly | > 0.10 | Alert, investigate |
| PSI | Monthly | > 0.25 | Retrain model |
| Gini | Quarterly | Drop > 10 pts | Root cause analysis |
| Bad rate by band | Monthly | ± 20% vs baseline | Review cutoffs |
| Exceptions (Basel) | Monthly | Yellow zone | Increase monitoring |
| Exceptions (Basel) | Monthly | Red zone | Model review |

---

## 6. Ethical Considerations

- Model không sử dụng các biến protected attributes (race, gender, religion)
- Tuổi được dùng nhưng theo cách neutral (experience proxy, không phải ageism)
- Reason codes được implement để đảm bảo quyền giải thích của khách hàng (ECOA compliance)
- Khuyến nghị human review cho các quyết định borderline (score 560–620)

---

## 7. Files & Artifacts

| File | Description |
|---|---|
| `model_lr.pkl` | Trained Logistic Regression model |
| `woe_bins.pkl` | WOE binning definitions |
| `scorecard_points.csv` | Scorecard lookup table |
| `cs_scores.csv` | Score for all training records |
| `psi_report.csv` | PSI analysis report |
| `model_metadata.json` | Model parameters & metrics |

---

*This model card follows the Google Model Cards framework (Mitchell et al., 2019)*
