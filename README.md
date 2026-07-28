# ValuMart Equity Analysis

A Python data analytics project examining pay equity across departments, gender, and regions at a fictional retail chain (ValuMart), using pandas and numpy in Google Colab.

## Project Overview

This notebook analyzes workforce, payroll, and store performance data for a 15 store retail chain. The core focus is a pay equity investigation: identifying departments where average hourly rates differ meaningfully by gender, quantifying the size of that gap, and estimating the annual dollar exposure if the gap were closed.

Note: I am not certain whether the underlying dataset is real company data, publicly available sample data, or synthetically generated for a course or practice exercise. If this matters for how you present the project (e.g. in an interview), you may want to confirm and state that origin explicitly in your own write up.

## Objective

1. Load and validate four related datasets (stores, employees, payroll, store activity)
2. Explore the workforce composition (department, employment type, gender, tenure, hire year)
3. Quantify pay differences between male and female employees within each department
4. Flag departments where the pay gap exceeds a set threshold
5. Estimate the annualized dollar exposure of the identified gap for affected employees
6. Explore secondary relationships: regional pay differences, store level sales and customer satisfaction, payroll cost by department and employment type

## Data

The notebook reads four CSV files (expected in the same working directory in Colab):

| File | Rows | Description |
|---|---|---|
| `dtStores.csv` | 15 | StoreID, Region, StoreSize, OpenedYear |
| `dtEmployees.csv` | 157 | EmployeeID, StoreID, Department, EmploymentType, Gender, HourlyRate, ContractedWeeklyHours, HireYear, YearsTenure |
| `dtPayroll.csv` | 4082 | PayrollID, EmployeeID, StoreID, PayPeriod, ActualHoursWorked, GrossPayment |
| `dtStoreActivity.csv` | 780 | StoreID, Week, WeeklySales, TransactionCount, CustomerSatisfactionScore |

None of the four datasets had missing values, confirmed with `.isna().sum()` checks early in the notebook.

## Tools and Libraries

- Python
- pandas
- numpy
- Google Colab (development environment)

## Analysis Performed

**Data validation**
Missing value checks across all four datasets.

**Descriptive exploration**
Value counts and summary statistics for gender, department, employment type, hourly rate, gross payment, and weekly sales. Employee sampling and groupby breakdowns by store, hire year, and department plus employment type.

**Pay gap analysis (core analysis)**
- Grouped average hourly rate by department and gender
- Calculated `payGapPct` as the percentage difference between male and female average hourly rate within each department: `(M minus F) / M times 100`
- Applied a 5 percent threshold to flag departments with a material gap
- Two departments were flagged: Management (16.57 percent) and Security (14.82 percent)

**Financial exposure estimate**
For the 15 female employees in the two flagged departments, the notebook calculates an `annualPayGap` per employee as `(male department average rate minus employee rate) times ContractedWeeklyHours times 52`, then sums this to a total annual pay gap exposure. The notebook reports this total as approximately $114,462.52. You may want to verify this figure independently if you plan to cite it, since it depends on the specific dataset used.

**Secondary analysis**
- Average tenure by department
- Gender distribution by store
- Average hourly rate by region (merging employee and store data)
- Highest paid Management employees
- Store level customer satisfaction and total sales rankings
- Weekly sales totals over time
- Total payroll cost by department and by employment type
- Crosstabs of department by gender (normalized) and region by store size

## Key Findings

- Two departments, Management and Security, show a gender pay gap above the 5 percent threshold, while Cashier, Customer Service, and Stocking show small or negative gaps favoring female employees
- The estimated annual pay gap exposure for affected female employees in those two departments is roughly $114,000, though this is a single dataset snapshot and should not be generalized without further validation
- Full time employees account for the large majority of total payroll cost ($4,416,729.00 versus $1,345,728.06 for part time, based on the notebook output)
- Regional average hourly rates vary, with Quebec and Alberta showing higher averages than Ontario and the Maritimes in this dataset

These figures come directly from the notebook's own printed output. I have not independently re-run or audited the underlying calculations, so if you plan to present these numbers publicly, it is worth re-running the notebook yourself to confirm.

## How to Run

1. Open the notebook in Google Colab (or Jupyter)
2. Place `dtStores.csv`, `dtEmployees.csv`, `dtPayroll.csv`, and `dtStoreActivity.csv` in the same directory as the notebook, or update the `pd.read_csv()` paths
3. Run all cells in order; later cells depend on variables created earlier (for example `pay_gap` and `flagged_depts` are used in the financial exposure section)

## Possible Extensions

If you want to build this out further for a portfolio, some natural next steps include:
- Statistical significance testing on the pay gap (for example a t test) rather than a fixed percentage threshold
- Controlling for tenure and contracted hours when comparing pay, since these could explain part of the gap
- Visualizations (bar charts of pay gap by department, a map or chart of regional differences)
- A short written summary memo aimed at a non technical audience (HR or leadership)

## Skills Demonstrated

Data cleaning and validation, groupby and aggregation, multi index unstacking, merging across relational tables, custom business logic for gap detection and financial impact estimation, and clear presentation of findings.
