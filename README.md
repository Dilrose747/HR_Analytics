# HR Analytics: Predicting Employee Attrition & Analyzing Retention Drivers

This repository showcases a complete **End-to-End Data Analytics Project** focused on **Employee Attrition Analysis**. The project uses a real-world HR dataset and applies advanced analytics techniques—ranging from Excel and SQL to Machine Learning and Power BI dashboards—to derive actionable insights for HR decision-making.

---

## Project Objectives

- Understand the factors leading to employee attrition.
- Build a predictive model to identify high-risk employees.
- Use data-driven insights to improve retention strategies.
- Deliver interactive dashboards for business stakeholders.

---

## Tools & Technologies Used

| Tool/Technology | Purpose |
|-----------------|---------|
| **Excel**       | Initial cleaning, grouping, feature engineering |
| **MySQL**       | Exploratory SQL analysis, joins, aggregates |
| **Python**      | Predictive modeling, EDA, SMOTE, Feature Selection |
| **Jupyter Notebook** | Model training and evaluation |
| **Scikit-learn / LightGBM / XGBoost** | ML models |
| **Power BI**    | Interactive dashboards and storytelling |
| **GitHub**      | Version control and portfolio presentation |

---

## Dataset Overview

**Source**: [`IBM HR Analytics Employee Attrition & Performance`]
**Format**: Combined Excel file (`.xlsx`) with one unified data sheet.
**Rows**: 1470 Employees' Details.

The dataset contains Columns with the following attributes:

- **Demographics**: Age, Gender, Education
- **Career Info**: Job Role, Department, Job Level, Total Working Years, Monthly Income
- **Engagement**: Job Involvement, Job Satisfaction, Work-Life Balance, Environment Satisfaction
- **Performance**: Performance Rating, Percent Salary Hike, Promotions
- **Attrition**: Binary target variable indicating whether the employee left

---

## 1. Excel Phase: Data Cleaning & Feature Engineering

- Removed duplicates and nulls.
- Created new categorical fields:
  - **Age Group**
  - **Tenure Level**
  - **Income Bracket**
- Filtered out outliers based on business rules.
- Built Pivot Tables to analyze the relationship between attrition and other factors.
- Exported cleaned file for further use in SQL and Power BI.

---

## 2. SQL Analysis Phase

Performed exploratory analysis using SQL:

- Aggregate KPIs:
  - Attrition % by department, gender, job role
  - Avg salary hike by education level
- Window functions for ranking employees by performance/salary.
- Used CTE's, JOINS, GROUP BY, HAVING, RANK(), ORDER BY functions.
- Cohort analysis for years at company vs attrition.

---

## 3. Python Phase: Modeling & Predictive Analytics

### Exploratory Data Analysis (EDA)

- Visualized attrition vs:
  - Age
  - Salary
  - Work-life balance
  - Satisfaction metrics
- Found that low satisfaction and salary hike strongly relate to attrition.

### Preprocessing & Feature Engineering

- Applied **Label Encoding** and **One-Hot Encoding**.
- Used **SMOTE** to handle class imbalance in attrition variable.
- Performed **Feature Selection** using:
  - `SelectKBest`
  - Feature Importance from Tree models

### Model Building

Trained and compared the following models:

| Model                | Metrics Used        |
|----------------------|---------------------|
| Logistic Regression  | Accuracy, Recall    |
| Decision Tree        | Accuracy, F1-Score  |
| Random Forest        | Accuracy, F1-Score  |
| XGBoost              | ROC-AUC             |
| LightGBM (Tuned)     | Accuracy, Recall    |

- Final model: **Logistic Regression (AUC=0.82)**.
- Achieved high **recall** and **balanced accuracy**, especially after using SMOTE and selecting relevant features.

---

## Results & Discussion

- **Satisfaction metrics** (job satisfaction, involvement, work-life balance) and **salary-related variables** (monthly income, salary hike %) were the most significant predictors.
- Employees with **low satisfaction and lower salary hikes** were more likely to leave.
- **Logistic Regression** (AUC = 0.82) — simple but effective, especially for linearly separable problems.
- **Random Forest** (Base, AUC = 0.79) — strong performance, less prone to overfitting than Decision Tree or boosted models in this case.
- **LightGBM and XGBoost** (Top Features) are comparable, but not superior to traditional Random Forest.
- **SMOTE + XGBoost** did not help and may have introduced noise or overfitting.
- **Decision Tree** performed the weakest—likely due to its high variance and lack of regularization.
- The model can be deployed to flag at-risk employees for HR intervention.

---

## Power BI Phase: Dashboard Development

Created two interactive dashboards in Power BI based on cleaned Excel data.

### 1. Attrition Overview Dashboard

- KPI Cards: Total Employees, Current Employees, Attrition Count, Attrition Rate.
- Visualizes overall attrition patterns by:
  - Department
  - Gender
  - Age Group
  - Monthly Income
  - Job Role
- Filters for gender, department, job-role, income-group.

### 2. Career & Retention Insights Dashboard

- Combines insights on:
  - Monthly Income vs Attrition
  - Job Satisfaction and Involvement
  - Salary Hike and Tenure Level
- KPIs highlighting average income, average satisfaction score.

**Purpose**: To help HR teams explore causes of attrition and spot high-risk employee segments.

---

## Key Takeaways

- **Low salary growth**, **lack of job satisfaction and environment satisfaction**, and **poor work-life balance** are major attrition drivers.
- Predictive analytics can help reduce attrition by identifying high-risk employees early.
- Data storytelling through dashboards enables non-technical stakeholders to make informed decisions.

---

## Skills Demonstrated

- Data Cleaning and Preprocessing
- SQL for Data Aggregation and Transformation
- Advanced Python Visualizations (EDA)
- Data analysis using Python
- Predictive Modeling
- DAX Measures and Power BI Design
- Dashboards and Data Storytelling

---

## Tech Stack

- Excel (Pivot Tables, Formulas)
- SQL (MySQL)
- Python (pandas, seaborn, sklearn, matplotlib)
- Power BI (DAX, Dashboards, Visualization)

---

## Acknowledgements

- Thanks to **IBM HR Analytics Employee Attrition & Performance** Dataset from Kaggle for providing a rich HR employee dataset ideal for end-to-end data analysis.

## Author

**Mohamed Dilrose P M**  
M.Sc. Statistics | Data Analyst  
[mohameddilrose2018@gmail.com]  
[https://www.linkedin.com/in/mohamed-dilrose-365554230/]  

---

## Tags

`#HRAnalytics` `#EmployeeAttrition` `#DataScienceProject` `#PowerBI` `#PythonML` `#SQLAnalytics` `#EndToEnd`

---


