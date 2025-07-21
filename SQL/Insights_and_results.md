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

| Attrition | Total Employees |
|-----------|-----------------|
| No        | 1233            |
| Yes       | 237             |

**Insight:**
> Out of a total of 1,470 employees, only **237 have left** the organization, while **1,233 have stayed**.
> - This implies an **attrition rate of ~16.1%**, which is moderate.
> - Continuous monitoring is essential to identify **patterns behind attrition** and address potential retention challenges.
> - Focused retention strategies may help reduce this attrition rate further.


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

| Department             | Total Employees | Attrition Count | Attrition Rate (%) |
|------------------------|------------------|------------------|---------------------|
| Sales                  | 446              | 92               | 20.63               |
| Research & Development | 961              | 133              | 13.84               |
| Human Resources        | 63               | 12               | 19.05               |

**Insight:**
> - The **Sales department** has the **highest attrition rate** at **20.63%**, followed closely by **Human Resources** at **19.05%**.
> - **Research & Development**, despite having the most employees, has the **lowest attrition rate** at **13.84%**.
> - This could indicate that R&D provides a more stable or engaging work environment.
> - The organization should further investigate **why Sales and HR have higher attrition**, potentially exploring workload, leadership, or compensation factors.


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

| Job Role                  | Average Monthly Income |
|---------------------------|------------------------|
| Manager                   | 17,181.68              |
| Research Director         | 16,033.55              |
| Healthcare Representative | 7,528.76               |
| Manufacturing Director    | 7,295.14               |
| Sales Executive           | 6,924.28               |
| Human Resources           | 4,235.75               |
| Research Scientist        | 3,239.97               |
| Laboratory Technician     | 3,237.17               |
| Sales Representative      | 2,626.00               |

**Insight:**
> - **Managers** and **Research Directors** earn significantly more than others, with average monthly incomes exceeding ₹17K and ₹16K respectively.
> - **Healthcare and Manufacturing roles** also show higher-than-average income levels.
> - Roles such as **Sales Representative**, **Research Scientist**, and **Lab Technician** earn the **lowest**, all below ₹3,300/month.
> - This disparity in compensation may influence **employee motivation and attrition**, especially in lower-income roles. Consider reviewing pay scales to ensure fair compensation and to improve employee retention.


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

| Gender | Overtime | Total Employees | Attrition Count | Attrition Rate (%) |
|--------|----------|------------------|------------------|---------------------|
| Female | No       | 408              | 40               | 9.80                |
| Female | Yes      | 180              | 47               | 26.11               |
| Male   | No       | 646              | 70               | 10.84               |
| Male   | Yes      | 236              | 80               | 33.90               |

**Insight:**
> - Employees working **overtime** face **significantly higher attrition rates**, especially **males (33.90%)** and **females (26.11%)**, compared to their non-overtime counterparts.
> - **Males with overtime** are the most vulnerable group with the highest attrition rate.
> - **Non-overtime employees** maintain much lower attrition rates across genders—below 11%.
>
> 🔍 **Recommendation**: Overtime could be a key driver of burnout and resignation. Companies should consider strategies such as:
> - Reducing excessive overtime
> - Implementing wellness programs
> - Offering better work-life balance to retain top talent


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

| Age | AgeGroup | Attrition | Department             | JobRole                | Gender | MonthlyIncome | OverTime | TotalWorkingYears | YearsAtCompany |
|-----|----------|-----------|------------------------|------------------------|--------|----------------|----------|-------------------|----------------|
| 41  | 40-50    | Yes       | Research & Development | Research Director      | Female | 19545          | No       | 23                | 22             |
| 58  | 50+      | Yes       | Research & Development | Healthcare Rep.        | Female | 10312          | No       | 40                | 40             |
| 32  | 30-40    | Yes       | Sales                  | Sales Executive        | Male   | 10400          | No       | 14                | 14             |
| 58  | 50+      | Yes       | Research & Development | Research Director      | Male   | 19246          | Yes      | 40                | 31             |
| 36  | 30-40    | Yes       | Sales                  | Sales Executive        | Male   | 10325          | Yes      | 16                | 16             |
| 37  | 30-40    | Yes       | Sales                  | Sales Executive        | Male   | 10609          | No       | 17                | 14             |
| 52  | 50+      | Yes       | Sales                  | Manager                | Female | 19845          | No       | 33                | 32             |
| 36  | 30-40    | Yes       | R&D                    | Lab Technician         | Female | 2743           | No       | 18                | 17             |
| 29  | <30      | Yes       | Sales                  | Sales Executive        | Female | 7336           | No       | 11                | 11             |
| 42  | 40-50    | Yes       | Sales                  | Sales Executive        | Male   | 13758          | Yes      | 22                | 21             |

**Insight:**
> - Attrition is diverse across departments but most commonly observed in Sales and Research & Development roles.
> - Job roles such as Sales Executive and Research Director appear repeatedly among those who left, indicating potential burnout or dissatisfaction in these high-demand positions.
> - Many of the employees who left have high monthly incomes (e.g., $19K+), suggesting compensation alone is not enough to retain senior employees.
> - Several employees had long tenures (20+ years), which may indicate either late-career transitions or a need to review retirement and exit planning.


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

| Job Role             | Total Employees | Attrition Count | Attrition Rate (%) |
|----------------------|------------------|------------------|---------------------|
| Sales Representative | 83               | 33               | 39.76               |
| Laboratory Technician| 259              | 62               | 23.94               |
| Human Resources      | 52               | 12               | 23.08               |
| Sales Executive      | 326              | 57               | 17.48               |
| Research Scientist   | 292              | 47               | 16.10               |

**Insight:**

> - **Sales Representatives face the highest attrition** at nearly **40%**, indicating job dissatisfaction or burnout.
> - **Laboratory Technicians and HR professionals** also have high attrition rates (~23%), suggesting department-level issues that may require managerial attention.
> - Although **Sales Executives and Research Scientists** have higher employee counts, their attrition rates are relatively lower, possibly due to better work conditions, support, or growth opportunities.
> - **Recommendation:** Prioritize employee engagement strategies for high-risk roles like Sales Representatives and Lab Technicians. Investigate job satisfaction, training needs, and team dynamics in these areas to improve retention.


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

| Department             | Attrition | Employees | Avg. Monthly Income |
|------------------------|-----------|-----------|----------------------|
| Human Resources        | No        | 51        | 7345.98              |
| Human Resources        | Yes       | 12        | 3715.75              |
| Research & Development | No        | 828       | 6630.33              |
| Research & Development | Yes       | 133       | 4108.08              |
| Sales                  | No        | 354       | 7232.24              |
| Sales                  | Yes       | 92        | 5908.46              |

**Insight:**
> - Across all departments, employees who **left the company (Attrition = Yes)** earned **significantly lower average monthly income** than those who stayed.
> - The income gap is particularly wide in **Human Resources**, where employees who left earned ~₹3.7K/month compared to ₹7.3K/month for those who stayed.
> - **Research & Development** shows the largest number of employees, and those who left earned ~₹2.5K/month less on average.
> - **Sales** employees who stayed earned more (~₹7.2K) than those who left (~₹5.9K), indicating a possible link between income and retention.

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

| Tenure Level | Employees | Attrition Count | Attrition Rate (%) |
|--------------|-----------|------------------|---------------------|
| <2 yrs       | 342       | 102              | 29.82               |
| 2-5 yrs      | 434       | 60               | 13.82               |
| 5+ yrs       | 694       | 75               | 10.81               |

**Insight:**
> - Employees with **less than 2 years of tenure** have the **highest attrition rate at nearly 30%**.
> - Attrition drops significantly to **13.82% for 2–5 years** and further to **10.81% for employees with over 5 years** of tenure.
> - This trend indicates a **critical attrition risk in early employment years**.

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

| Education Field     | Avg Monthly Income | Employees | Attrition Count | Attrition Rate (%) |
|---------------------|--------------------|-----------|------------------|---------------------|
| Human Resources     | 7241.15            | 27        | 7                | 25.93               |
| Technical Degree    | 5758.30            | 132       | 32               | 24.24               |
| Marketing           | 7348.58            | 159       | 35               | 22.01               |
| Life Sciences       | 6463.29            | 606       | 89               | 14.69               |
| Medical             | 6510.04            | 464       | 63               | 13.58               |
| Other               | 6071.55            | 82        | 11               | 13.41               |

**Insight:**
> - **Human Resources** has the **highest attrition rate (25.93%)**, though it has the fewest employees.
> - **Technical Degree** and **Marketing** fields also face high attrition, over **22%**, despite competitive average salaries.
> - **Life Sciences**, **Medical**, and **Other** education fields maintain **lower attrition rates**, all around **13–15%**.


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

| Job Role               | Total Employees | Attrition Count | Attrition Rate (%) |
|------------------------|-----------------|------------------|---------------------|
| Sales Representative   | 83              | 33               | 39.76               |
| Laboratory Technician  | 259             | 62               | 23.94               |
| Human Resources        | 52              | 12               | 23.08               |
| Sales Executive        | 326             | 57               | 17.48               |
| Research Scientist     | 292             | 47               | 16.10               |

**Insight:**
> - **Sales Representatives have the highest attrition rate (39.76%)**, which is a significant concern given the customer-facing nature of the role.
> - **Laboratory Technicians** and **Human Resources** roles also show high attrition rates, above **23%**.
> - Though **Sales Executives** and **Research Scientists** have lower attrition rates, they're still notable and worth monitoring.
>
> High attrition in sales and technical roles can impact productivity, team stability, and customer relationships.


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

| Department             | Job Role                | Average Income | Role Rank |
|------------------------|--------------------------|----------------|-----------|
| Human Resources        | Manager                  | 18,088.64      | 1         |
| Human Resources        | Human Resources          | 4,235.75       | 2         |
| Research & Development | Manager                  | 17,130.33      | 1         |
| Research & Development | Research Director        | 16,033.55      | 2         |
| Research & Development | Healthcare Representative| 7,528.76       | 3         |
| Research & Development | Manufacturing Director   | 7,295.14       | 4         |
| Research & Development | Research Scientist       | 3,239.97       | 5         |
| Research & Development | Laboratory Technician    | 3,237.17       | 6         |
| Sales                  | Manager                  | 16,986.97      | 1         |
| Sales                  | Sales Executive          | 6,924.28       | 2         |
| Sales                  | Sales Representative     | 2,626.00       | 3         |

**Insight:**
> - **Managerial roles** consistently earn the highest incomes across all departments, showing the expected seniority-pay correlation.
> - **Significant pay gaps** exist within departments:
>   - In **Human Resources**, Managers earn over **4x** more than other roles.
>   - In **R&D**, top roles like Managers and Research Directors earn **double or more** than technical roles.
>   - In **Sales**, Managers earn over **6x** more than Sales Representatives.


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

| Employee Number | Department             | Job Role          | Monthly Income | Dept Avg Income | Income Difference |
|-----------------|------------------------|-------------------|----------------|------------------|--------------------|
| 520             | Research & Development | Manager           | 19,999         | 6,281.25         | 13,717.75          |
| 983             | Research & Development | Research Director | 19,973         | 6,281.25         | 13,691.75          |
| 1081            | Research & Development | Manager           | 19,943         | 6,281.25         | 13,661.75          |
| 408             | Research & Development | Manager           | 19,926         | 6,281.25         | 13,644.75          |
| 760             | Research & Development | Manager           | 19,859         | 6,281.25         | 13,577.75          |
| 1125            | Research & Development | Research Director | 19,740         | 6,281.25         | 13,458.75          |
| 1475            | Research & Development | Research Director | 19,701         | 6,281.25         | 13,419.75          |
| 1794            | Research & Development | Research Director | 19,665         | 6,281.25         | 13,383.75          |
| 1107            | Research & Development | Research Director | 19,627         | 6,281.25         | 13,345.75          |
| 1421            | Research & Development | Research Director | 19,626         | 6,281.25         | 13,344.75          |
| 1549            | Research & Development | Manager           | 19,613         | 6,281.25         | 13,331.75          |
| 526             | Research & Development | Manager           | 19,566         | 6,281.25         | 13,284.75          |
| 36              | Research & Development | Research Director | 19,545         | 6,281.25         | 13,263.75          |
| 190             | Research & Development | Research Director | 19,537         | 6,281.25         | 13,255.75          |
| 566             | Research & Development | Manager           | 19,513         | 6,281.25         | 13,231.75          |
| 493             | Research & Development | Research Director | 19,502         | 6,281.25         | 13,220.75          |
| 243             | Research & Development | Research Director | 19,436         | 6,281.25         | 13,154.75          |

**Insight:**
> - These 17 employees from **Research & Development** earn significantly more than the **department average**.
> - All of them are **Managers** or **Research Directors**, roles that are evidently compensated at the uppermost level.
> - Each of them earns over **13,000 more than the average**, suggesting that their salaries are outliers within the department.


# 13. Loyalty Ranking
-- Business Question:
--     Who are the most loyal employees in each department?
--
-- Why:
--     Can be considered for leadership and mentoring roles.
-- ---------------------------------------------------------
SELECT EmployeeNumber, Department, YearsAtCompany,
       RANK() OVER (PARTITION BY Department ORDER BY YearsAtCompany DESC) AS LoyaltyRank
FROM employee_attrition;

| Employee Number | Department       | Years At Company | Loyalty Rank |
|-----------------|------------------|------------------|---------------|
| 1277            | Human Resources  | 33               | 1             |
| 498             | Human Resources  | 32               | 2             |
| 691             | Human Resources  | 22               | 3             |
| 1425            | Human Resources  | 21               | 4             |
| 684             | Human Resources  | 21               | 4             |
| 1140            | Human Resources  | 20               | 6             |
| 1422            | Human Resources  | 20               | 6             |
| 1744            | Human Resources  | 11               | 8             |
| 298             | Human Resources  | 11               | 8             |
| 1447            | Human Resources  | 10               | 10            |
| 1210            | Human Resources  | 10               | 10            |
| 1703            | Human Resources  | 10               | 10            |
| 1646            | Human Resources  | 10               | 10            |

**Insight:**
> - These 13 employees from the **Human Resources** department represent the **most loyal segment**, ranked by **years at the company**.
> - The top two employees, **Employee #1277 and #498**, have served for **over 30 years**, indicating deep institutional knowledge.
> - Ties in **YearsAtCompany** are reflected in shared **Loyalty Ranks**, e.g., four employees ranked **10th** have 10 years of service.


# 14. High Earners Above Department Average
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

| Employee Number | Department              | Monthly Income |
|-----------------|-------------------------|----------------|
| 33              | Sales                   | ₹8,726         |
| 36              | Research & Development  | ₹19,545        |
| 51              | Research & Development  | ₹9,884         |
| 54              | Research & Development  | ₹13,458        |
| 60              | Research & Development  | ₹9,526         |
| 62              | Sales                   | ₹9,069         |
| 74              | Human Resources         | ₹18,844        |
| 76              | Research & Development  | ₹18,740        |
| 78              | Sales                   | ₹7,637         |
| 79              | Research & Development  | ₹10,096        |
| 81              | Research & Development  | ₹18,172        |
| 83              | Research & Development  | ₹14,756        |
| 84              | Research & Development  | ₹6,499         |
| 88              | Research & Development  | ₹9,724         |
| 100             | Research & Development  | ₹9,980         |
| 103             | Research & Development  | ₹7,484         |

**Insight:**
> - **R&D Department** has the **widest income range**. Investigate differences in job roles, experience, or performance metrics to ensure fairness.
> - **Sales Department** salaries are relatively consistent and within a narrow band.


