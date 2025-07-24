# HR Analytics with Python: Predicting Employee Attrition

This section of the project focuses on building predictive models using Python to determine the likelihood of employee attrition based on various organizational and personal factors.

---

##  Objectives

- Perform EDA and feature engineering on HR attrition data.
- Build predictive models (Logistic Regression, Random Forest, XGBoost, LightGBM).
- Apply techniques like SMOTE for imbalance handling.
- Compare models using metrics like precision, recall, F1-score, and AUC.
- Select the best-performing model for deployment.

---

## ⚙ Technologies Used

- **Python 3.10+**
- `pandas`, `numpy` – Data manipulation
- `matplotlib`, `seaborn` – Visualization
- `scikit-learn` – ML models and evaluation
- `xgboost`, `lightgbm` – Advanced tree-based models
- `imblearn` – SMOTE for imbalanced classification

---

##  Steps Performed

### 1. Data Preprocessing
- Handled categorical variables using one-hot encoding
- Addressed class imbalance using **SMOTE**
- Selected top features using `SelectKBest` and model-based importance

### 2. Model Training & Evaluation
| Model Variant                 | Techniques Applied                  |
|------------------------------|-------------------------------------|
| Logistic Regression           | Baseline model                     |
| Decision Tree                 | Gini-based tree classifier         |
| Random Forest (base & tuned) | Feature importance, SMOTE variants |
| XGBoost                      | SMOTE + hyperparameter tuning      |
| LightGBM                     | Fast gradient boosting             |

### 3. Metrics Tracked
- **Precision, Recall, F1-score** (focused on attrition class = 1)
- **AUC-ROC Score** for probability-based comparison
- **Confusion Matrix** to understand error types

### 4. Final Model Selection
- Compared using performance table and ROC curves
- Logistic Regression and Random Forest emerged as top candidates based on use case

---

##  ROC Curve Comparison

> ROC curves plotted using `roc_auc_score` and `RocCurveDisplay` from `sklearn.metrics`  
> All models compared on same test data to ensure fairness.

---
