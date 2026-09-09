# HR Analytics: Salary, Training & Turnover

> **People Analytics Project · Confidential**
> An end-to-end analysis of **2,000 employee records** to evaluate pay equity, training ROI, and employee turnover risk.

---

## 1. Executive Summary

This project analyzes workforce data to answer three leadership questions:

1. **Is employee pay equitable?**
2. **Did the Q3 sales-training program generate a meaningful return?**
3. **When and why are employees leaving?**

### Key Findings

| Area                         | Finding                                         |
| ---------------------------- | ----------------------------------------------- |
| Workforce                    | 2,000 employees across 6 departments            |
| Average salary               | **$88,018**                                     |
| Gender pay test              | **Not statistically significant** (`p = 0.214`) |
| Training revenue lift        | **+$12,027 per trained employee**               |
| Training ROI                 | **~47×**                                        |
| 2-year attrition probability | **41.3%**                                       |
| 5-year attrition probability | **73.6%**                                       |
| Average satisfaction         | **3.0 / 5**                                     |

---

# 2. Project Architecture

The analysis combines five workforce datasets into a single analytical workflow.

```mermaid
flowchart LR
    A[Employee Directory<br/>2,000 records]
    B[Compensation & Demographics<br/>2,000 records]
    C[Messy Performance Data<br/>400 records]
    D[Training A/B Test<br/>400 employees]
    E[Attrition Distributions<br/>500 exits]

    A --> F[Data Integration]
    B --> F
    C --> G[Power Query Cleaning]
    G --> F
    D --> F
    E --> F

    F --> H[Integrated HR Dataset]

    H --> I[Salary Equity]
    H --> J[Training ROI]
    H --> K[Turnover Risk]

    I --> L[Leadership Recommendations]
    J --> L
    K --> L
```

### Data Sources

| Dataset                     |   Records | Purpose                                  |
| --------------------------- | --------: | ---------------------------------------- |
| `Employee_Directory`        |     2,000 | Department, role level, hire date        |
| `Compensation_Demographics` |     2,000 | Salary, bonus, age, gender, satisfaction |
| `Messy_Performance_Data`    | 400 → 352 | Performance data requiring cleaning      |
| `Training_AB_Test`          |       400 | Control vs. training revenue             |
| `Attrition_Distributions`   | 500 exits | Tenure, exit reason, rehire eligibility  |

The source presentation identifies XLOOKUP/INDEX-MATCH, Power Query, pivot tables, descriptive statistics, t-tests, ANOVA, chi-square, and probability models as the main analytical methods.

---

# 3. Data Preparation

## Cleaning Pipeline

```mermaid
flowchart TD
    A[Raw Performance Data]
    --> B[Trim Whitespace]

    B --> C[Proper Case]
    C --> D[Filter Invalid Records]
    D --> E[352 Clean Records]

    E --> F[Merge with Workforce Data]
    F --> G[Analysis-Ready Dataset]
```

### Integration Strategy

Employee-level information was merged using lookup-based techniques:

```mermaid
flowchart LR
    A[Employee ID]
    --> B{Lookup Match}

    B --> C[Department]
    B --> D[Role Level]
    B --> E[Salary]
    B --> F[Gender]
    B --> G[Satisfaction]
    B --> H[Performance]
```

---

# 4. Analytical Framework

```mermaid
flowchart TD
    A[Integrated Workforce Data]

    A --> B[Salary Analysis]
    A --> C[Training Experiment]
    A --> D[Attrition Analysis]

    B --> B1[Department Comparison]
    B --> B2[Role-Level Comparison]
    B --> B3[Gender Pay Test]

    C --> C1[Control Group]
    C --> C2[Training Group]
    C --> C3[Revenue Comparison]
    C --> C4[Two-Sample t-Test]
    C --> C5[ROI Calculation]

    D --> D1[Tenure Distribution]
    D --> D2[Attrition Probability]
    D --> D3[Exit Reasons]
    D --> D4[Rehire Eligibility]
    D --> D5[Department Satisfaction]

    B3 --> E[Leadership Decisions]
    C5 --> E
    D5 --> E
```

---

# 5. Salary Equity Analysis

## Salary by Role

The strongest salary pattern is associated with **role seniority**.

| Role      | Average Base Salary |
| --------- | ------------------: |
| Junior    |             $49,937 |
| Mid-Level |             $79,736 |
| Senior    |            $109,633 |
| Lead      |            $129,934 |
| Manager   |            $149,997 |
| Director  |            $200,114 |

```mermaid
flowchart LR
    A[Junior<br/>$49,937]
    --> B[Mid-Level<br/>$79,736]
    --> C[Senior<br/>$109,633]
    --> D[Lead<br/>$129,934]
    --> E[Manager<br/>$149,997]
    --> F[Director<br/>$200,114]
```

### Interpretation

The presentation identifies an approximately **4× salary spread** between Junior and Director roles, while the department-level spread is considerably smaller. This indicates that **seniority is the stronger salary driver** in the analyzed dataset.

---

# 6. Gender Pay Gap Test

A Welch's two-sample t-test compared male and female base salaries.

| Metric      |    Male |  Female |
| ----------- | ------: | ------: |
| Sample size |     989 |     933 |
| Mean salary | $87,271 | $89,290 |

### Statistical Result

```mermaid
flowchart TD
    A[Gender Salary Comparison]
    --> B[Welch's Two-Sample t-Test]

    B --> C[t = -1.242]
    B --> D[p = 0.214]
    B --> E[α = 0.05]

    D --> F{p > α?}

    F -->|Yes| G[Fail to Reject H₀]
    G --> H[No statistically significant<br/>aggregate gender pay gap]
```

### Conclusion

Because **p = 0.214 > 0.05**, the analysis does not find statistically significant evidence of a gender pay gap at the aggregate level.

However, aggregate parity does **not** eliminate the possibility of differences within individual roles or departments. The recommended next step is therefore to repeat the analysis using role-level and department-level slices.

---

# 7. Training ROI Analysis

## Experimental Design

The Q3 sales-training program was evaluated using two groups:

```mermaid
flowchart TD
    A[400 Sales Employees]
    --> B{A/B Test}

    B --> C[Control<br/>200 employees]
    B --> D[Training<br/>200 employees]

    C --> E[Avg Q3 Revenue<br/>$97,902]
    D --> F[Avg Q3 Revenue<br/>$109,929]

    E --> G[Revenue Difference]
    F --> G

    G --> H[+$12,027 per employee]
```

The training group generated an average Q3 revenue of **$109,929**, compared with **$97,902** for the control group.

---

## Statistical Validation

```mermaid
flowchart TD
    A[Training vs Control Revenue]
    --> B[Two-Sample t-Test]

    B --> C[t = 5.62]
    B --> D[df ≈ 395]
    B --> E[p < 0.0001]

    E --> F{p < 0.05?}

    F -->|Yes| G[Reject H₀]
    G --> H[Revenue Lift is<br/>Statistically Significant]
```

The result is highly statistically significant (`p < 0.0001`), supporting the conclusion that the observed revenue lift is unlikely to be explained by random variation alone.

---

# 8. Training ROI Calculation

### Investment

**Program cost:** $50,000

### Incremental Revenue

```text
$12,027 × 200 employees
≈ $2,405,471 incremental revenue
```

### ROI

```mermaid
flowchart LR
    A[$50K Training Investment]
    --> B[200 Employees]

    B --> C[+$12,027 Average Revenue Lift]

    C --> D[$2.405M Incremental Revenue]

    D --> E[~47× ROI]
```

The presentation calculates approximately **47× return on investment** and recommends expanding the training program.

---

# 9. Turnover Risk

## Attrition Model

The turnover analysis models **500 exits** using an exponential distribution.

```mermaid
flowchart TD
    A[500 Employee Exits]
    --> B[Exponential Distribution]

    B --> C[Mean Exit Tenure<br/>3.75 years]
    B --> D[λ = 0.267]

    C --> E[Attrition Probability]
    D --> E

    E --> F[Year 1: 23.4%]
    E --> G[Year 2: 41.3%]
    E --> H[Year 3: 55.0%]
    E --> I[Year 5: 73.6%]
    E --> J[Year 10: 93.0%]
```

---

## Retention Risk Timeline

```mermaid
timeline
    title Cumulative Probability of Employee Exit
    Year 1 : 23.4%
    Year 2 : 41.3%
    Year 3 : 55.0%
    Year 5 : 73.6%
    Year 10 : 93.0%
```

The model estimates a **41.3% probability that a new hire exits within the first two years**. By year five, the modeled cumulative probability reaches **73.6%**.

> **Important modeling note:** The exponential model assumes a constant hazard rate. Therefore, these probabilities describe the fitted model rather than proving that actual employee risk is constant throughout tenure.

---

# 10. Why Employees Leave

The 500 exits were distributed across four recorded reasons:

```mermaid
pie title Reasons for Leaving
    "Career Change" : 27.4
    "Better Offer" : 24.6
    "Commute" : 24.8
    "Management" : 23.2
```

The exit reasons are relatively evenly distributed rather than being dominated by one category.

---

# 11. Additional Statistical Tests

## Exit Reason vs. Rehire Eligibility

```mermaid
flowchart LR
    A[Exit Reason]
    --> B[Chi-Square Test]

    B --> C[p = 0.296]

    C --> D{p > 0.05?}
    D -->|Yes| E[Not Significant]

    E --> F[Exit reason does not<br/>predict rehire eligibility]
```

## Satisfaction Across Departments

```mermaid
flowchart LR
    A[Job Satisfaction]
    --> B[One-Way ANOVA]

    B --> C[p = 0.35]

    C --> D{p > 0.05?}
    D -->|Yes| E[Not Significant]

    E --> F[Satisfaction is not<br/>concentrated in one department]
```

The presentation reports an average satisfaction score of **3.0/5** and finds no statistically significant departmental difference (`p = 0.35`).

---

# 12. Overall Findings

```mermaid
mindmap
  root((HR Analytics))
    Salary Equity
      Role level is strongest driver
      No significant aggregate gender gap
      Continue role-level monitoring
    Training ROI
      +$12,027 revenue per employee
      p < 0.0001
      ~47× ROI
      Expand program
    Turnover
      41.3% modeled 2-year risk
      73.6% modeled 5-year risk
      Exit reasons broadly distributed
      Satisfaction consistent across departments
```

---

# 13. Recommendations

## 1. Scale the Training Program

The Q3 training pilot produced a reported **~47× ROI**.

**Action:**

* Expand the training program.
* Track revenue lift by cohort.
* Continue measuring trained vs. untrained employees.

---

## 2. Front-Load Retention Investment

The modeled probability of leaving reaches **41.3% by year two**.

**Action:**

* Strengthen onboarding.
* Introduce structured mentorship.
* Increase manager check-ins during the first 24 months.
* Monitor early-tenure turnover.

---

## 3. Re-Test Pay Equity Annually

The current aggregate gender comparison is not statistically significant.

**Action:**

* Repeat the test annually.
* Analyze salary by role level.
* Analyze salary by department.
* Monitor changes as workforce composition changes.

---

## 4. Add Qualitative Exit Analysis

Structured exit reasons alone may not capture the full employee experience.

**Action:**

* Improve exit interviews.
* Categorize qualitative responses.
* Compare qualitative themes with compensation, workload, management, and tenure.
* Look for emerging retention drivers.

The presentation recommends qualitative investigation because structured exit data may not capture drivers such as compensation timing or workload.

---

# 14. Decision Framework

```mermaid
flowchart TD
    A[HR Analytics Findings]

    A --> B{Training ROI}
    B -->|~47×| C[Scale Training]

    A --> D{2-Year Attrition Risk}
    D -->|41.3%| E[Prioritize Early-Tenure Retention]

    A --> F{Gender Pay Test}
    F -->|p = 0.214| G[Monitor & Re-Test]

    A --> H{Department Satisfaction}
    H -->|p = 0.35| I[Avoid Department-Wide Retention Assumptions]

    C --> J[Leadership Action Plan]
    E --> J
    G --> J
    I --> J
```

---

# 15. Technology & Methods

```mermaid
flowchart LR
    A[Raw Data]
    --> B[Power Query]

    B --> C[Excel / Data Integration]
    C --> D[Pivot Tables]

    D --> E[Descriptive Statistics]

    E --> F[t-Test]
    E --> G[ANOVA]
    E --> H[Chi-Square]
    E --> I[Probability Models]

    F --> J[Business Recommendations]
    G --> J
    H --> J
    I --> J
```

### Methods Used

* XLOOKUP / INDEX-MATCH
* Power Query
* Data cleaning
* Pivot tables
* Descriptive statistics
* Welch's two-sample t-test
* Two-sample t-test
* One-way ANOVA
* Chi-square test
* Exponential probability model
* Binomial probability model

---

# 16. Final Takeaway

```mermaid
flowchart TD
    A[2,000 Employees]
    --> B[Three Business Questions]

    B --> C[Is Pay Fair?]
    B --> D[Does Training Pay?]
    B --> E[Why Do People Leave?]

    C --> F[No Aggregate Gender Gap Detected]
    D --> G[~47× Training ROI]
    E --> H[Focus Retention on First 24 Months]

    F --> I[Monitor]
    G --> J[Scale]
    H --> K[Retain]

    I --> L[People Analytics Action Plan]
    J --> L
    K --> L
```

> **Bottom line:** The analysis points to three immediate priorities: **scale what works, invest earlier in retention, and continue monitoring equity at a more granular level.**

---

## Source

**HR Analytics Presentation — Salary, Training & Turnover**

The presentation states that full detail, including live formulas, pivot tables, and statistical tests, is available in the companion workbook `HR_Analytics_Solved.xlsx`.

