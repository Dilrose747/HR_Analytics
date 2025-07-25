# HR Analytics Power BI Dashboard – Employee Attrition & Retention

This repository contains the Power BI `.pbix` file for my end-to-end HR analytics project. The goal of this analysis is to understand employee attrition patterns and provide data-driven insights for improving employee retention and engagement.

---

## Project Objectives

- Analyze **employee attrition patterns** across departments, age groups, salary brackets, and job roles.
- Explore how **salary growth, career progression, job satisfaction**, and **work-life balance** influence employee retention.
- Build **interactive dashboards** in Power BI for HR decision-makers.

---

##  Tools & Techniques

| Tool          | Usage                                       |
|---------------|---------------------------------------------|
| **Power BI**  | Dashboard creation and visualization        |
| **Power Query** | Data cleaning and transformation           |
| **DAX**       | Calculated columns, measures, KPI metrics   |

---

## Dashboard Overview

### 1. Employee Attrition Overview Dashboard

> **Objective**: Provide an at-a-glance view of overall attrition distribution across key employee dimensions.

**Key Features**:
- Total Employees, Attrition Count and Rate KPI cards.
- Attrition by **Department**, **Gender**, **Job Role**, and **Income Group** (Bar Charts).
- Attrition rate by **Age** (Line Chart).
- Slicers for filtering by **Attrition**, **Gender**, **Department**, **Income Group**, and **Job Role**.
- Highlights of high-attrition departments and roles.

---

### 2. Career & Retention Insights Dashboard

> **Objective**: Deep dive into how salary, satisfaction, and engagement influence employee attrition.

**Key Features**:
- Comparative bar charts of **Attrition vs Job Satisfaction**, **Environment Satisfaction**, and **Work-Life Balance**.
- Trend analysis of **Salary Hike % vs Attrition Count**.
- KPIs showing **Average Monthly Income**, **Avg Job Satisfaction**, **Avg Environment Satisfaction**, and **Avg Work-Life Balance**.
- Multiple Line Charts of **Income growth** by **Job Role** and **Experience**

---

## Key Metrics Created (via DAX)

- Attrition Rate = DIVIDE(CALCULATE(COUNTROWS('HR-Employee-Attriti'), 'HR-Employee-Attriti'[Attrition]="Yes"), COUNTROWS('HR-Employee-Attriti'))
- Total Employees = COUNTROWS('HR-Employee-Attrition')
- Current Employees = CALCULATE(COUNTROWS('HR-Employee-Attriti'), 'HR-Employee-Attriti'[Attrition] = "No")
- Attrition Count = CALCULATE(COUNTROWS('HR-Employee-Attriti'), 'HR-Employee-Attriti'[Attrition] = "Yes")
- Avg Monthly Income = AVERAGE(MonthlyIncome)
- Avg JobSatisfaction = AVERAGE(JobSatisfaction)
- Avg Environment Satisfaction = AVERAGE(EnvironmentSatisfaction)
- Avg Work-Life Balance = AVERAGE(WorkLifeBalance)

---

## Author

**Mohamed Dilrose**  
M.Sc. in Statistics | Data Analyst

---


