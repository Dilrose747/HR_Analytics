# 1. Overall Attrition Count
-- Business Question:
--     What is the overall attrition rate in the company?
--
-- Why:
--     Helps HR understand the magnitude of employee loss.
-- ---------------------------------------------------------
SELECT Attrition, COUNT(*) AS total_employees
FROM employee_attrition
GROUP BY Attrition;

# 2. Attrition by Department
-- Business Question:
--     Which departments experience the highest attrition rates?
--
-- Why:
--     Identifies departments with retention challenges.
-- ---------------------------------------------------------
SELECT Department,
       COUNT(*) AS total,
       SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END) AS attrition_count,
       ROUND(SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END)*100.00/COUNT(*), 2) AS attrition_rate
FROM employee_attrition
GROUP BY Department;

# 3. Average Income by Job Role
-- Business Question:
--     What is the average monthly income for each job role?
--
-- Why:
--     Reveals income disparity and compensation planning.
-- ---------------------------------------------------------
SELECT JobRole,
       ROUND(AVG(MonthlyIncome), 2) AS AverageMonthlyIncome
FROM employee_attrition
GROUP BY JobRole
ORDER BY AverageMonthlyIncome DESC;

# 4. Gender & Overtime vs. Attrition
-- Business Question:
--     Is there a correlation between gender, overtime, and attrition?
--
-- Why:
--     Assesses fairness and workload balance across demographics.
-- ---------------------------------------------------------
SELECT Gender, Overtime,
       COUNT(*) AS TotalEmployees,
       SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END) AS AttritionCount,
       ROUND(SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END)*100.00/COUNT(Attrition), 2) AS AttritionRate
FROM employee_attrition
GROUP BY Gender, Overtime
ORDER BY Gender;

# 5. Long-Term Employees Who Left
-- Business Question:
--     How many long-tenure employees (10+ years) still left the company?
--
-- Why:
--     Important for understanding issues even among loyal employees.
-- ---------------------------------------------------------
SELECT *
FROM employee_attrition
WHERE YearsAtCompany > 10 AND Attrition = "Yes";

# 6. Top 5 Roles with Highest Attrition
-- Business Question:
--     Which job roles have the highest attrition rates?
--
-- Why:
--     Helps in role-specific retention strategies.
-- ---------------------------------------------------------
SELECT JobRole,
       COUNT(*) AS TotalEmployees,
       SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END) AS AttritionCount,
       ROUND(SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END)*100.00/COUNT(*), 2) AS AttritionRate
FROM employee_attrition
GROUP BY JobRole
ORDER BY AttritionRate DESC
LIMIT 5;

# 7. Income by Department & Attrition Status
-- Business Question:
--     How does income vary across departments based on attrition?
--
-- Why:
--     Shows whether income is a driving factor for attrition in specific departments.
-- ---------------------------------------------------------
SELECT Department,
       Attrition,
       COUNT(*) AS Employees,
       ROUND(AVG(MonthlyIncome), 2) AS AvgIncome
FROM employee_attrition
GROUP BY Department, Attrition
ORDER BY Department, Attrition;

# 8. Attrition by Tenure Level
-- Business Question:
--     How does employee tenure affect attrition?
--
-- Why:
--     Reveals risk periods during an employee's lifecycle.
-- ---------------------------------------------------------
SELECT TenureLevel,
       COUNT(*) AS Employees,
       SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END) AS AttritionCount,
       ROUND(SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END)*100.00/COUNT(*), 2) AS AttritionRate
FROM employee_attrition
GROUP BY TenureLevel
ORDER BY TenureLevel;

# 9. Education Field vs. Attrition & Income
-- Business Question:
--     Which education backgrounds are more prone to attrition?
--
-- Why:
--     Helps in targeting learning programs and reskilling needs.
-- ---------------------------------------------------------
SELECT EducationField,
       AVG(MonthlyIncome) AS AvgMonthlyIncome,
       COUNT(*) AS Employees,
       SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END) AS AttritionCount,
       ROUND(SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END)*100.00/COUNT(*), 2) AS AttritionRate
FROM employee_attrition
GROUP BY EducationField
ORDER BY AttritionRate DESC;

# 10. Top 5 Roles with Highest Attrition (Using CTE)
-- Business Question:
--     Repeating earlier attrition analysis using CTEs for clarity.
--
-- Why:
--     Demonstrates use of modern SQL structuring for modular analytics.
-- ---------------------------------------------------------
WITH AttritionStats AS (
    SELECT JobRole,
           COUNT(*) AS TotalEmployees,
           SUM(CASE WHEN Attrition = "Yes" THEN 1 ELSE 0 END) AS AttritionCount
    FROM employee_attrition
    GROUP BY JobRole
),
AttritionRateCalc AS (
    SELECT JobRole,
           TotalEmployees,
           AttritionCount,
           ROUND(AttritionCount*100.00/TotalEmployees, 2) AS AttritionRate
    FROM AttritionStats
)
SELECT *
FROM AttritionRateCalc
ORDER BY AttritionRate DESC
LIMIT 5;

# 11. Income Ranking by Role within Department
-- Business Question:
--     What are the highest-paid job roles within each department?
--
-- Why:
--     Useful for budget planning and internal equity.
-- ---------------------------------------------------------
SELECT Department, JobRole, ROUND(AVG(MonthlyIncome), 2) AS AvgIncome,
       RANK() OVER (PARTITION BY Department ORDER BY AVG(MonthlyIncome) DESC) AS RoleRank
FROM employee_attrition
GROUP BY Department, JobRole;

# 12. Employee vs. Department Average Income
-- Business Question:
--     Which employees earn more or less than their department average?
--
-- Why:
--     Useful for identifying pay gaps and outliers.
-- ---------------------------------------------------------
SELECT EmployeeNumber, Department, JobRole, MonthlyIncome,
       ROUND(AVG(MonthlyIncome) OVER (PARTITION BY Department), 2) AS DeptAvgIncome,
       ROUND(MonthlyIncome - AVG(MonthlyIncome) OVER (PARTITION BY Department), 2) AS IncomeDifference
FROM employee_attrition
ORDER BY IncomeDifference DESC;

-- 13. Loyalty Ranking
-- Business Question:
--     Who are the most loyal employees in each department?
--
-- Why:
--     Can be considered for leadership and mentoring roles.
-- ---------------------------------------------------------
SELECT EmployeeNumber, Department, YearsAtCompany,
       RANK() OVER (PARTITION BY Department ORDER BY YearsAtCompany DESC) AS LoyaltyRank
FROM employee_attrition;

-- 14. High Earners Above Department Average
-- Business Question:
--     Which employees earn more than their department average?
--
-- Why:
--     Identifies high performers or overpaid individuals for review.
-- ---------------------------------------------------------
SELECT h1.EmployeeNumber, h1.Department, h1.MonthlyIncome
FROM employee_attrition h1
JOIN (
    SELECT Department, AVG(MonthlyIncome) AS DeptAvg
    FROM employee_attrition
    GROUP BY Department
) h2
ON h1.Department = h2.Department
WHERE h1.MonthlyIncome > h2.DeptAvg;
