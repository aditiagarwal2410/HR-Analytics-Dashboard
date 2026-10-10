# HR Analytics Dashboard in Power BI

An end-to-end HR analytics project in Power BI that analyses 2,845 employee records to understand where attrition concentrates, how engaged and productive the workforce is, and whether training spend is working. It ends in specific, evidence-based recommendations for HR.

Built for the course *AI for Managers: Application and Strategy* at FORE School of Management.

---

## Business problem

Organisations hold rich employee data but rarely have one place to answer basic questions:

- Where is attrition concentrated?
- Which employee segments are disengaged?
- Is training spend producing results?

This project builds a clean data model and a verified set of measures so these questions can be answered consistently and repeatedly.

## Objectives

- **Workforce analysis:** headcount by department, division, geography and demographics
- **Attrition analysis:** overall attrition and attrition by segment
- **Diversity analysis:** gender, race, marital status and age
- **Engagement and performance:** engagement, satisfaction, work-life balance and performance, and how each relates to attrition
- **Training effectiveness:** training cost, success rate and outcomes
- **Recommendations:** turn findings into concrete HR actions

## Dataset

- **Source:** [HR Analytics Dataset on Kaggle](https://www.kaggle.com/datasets/hopesb/hr-analytics-dataset)
- **Size:** 2,845 rows, one per employee, keyed on Employee ID
- **Fields:** 32 in total (28 source columns plus 4 derived fields)
- **Quality:** 0 missing values and 0 duplicate Employee IDs after cleaning

The fields cover identity and role, geography, key dates (start, birth, survey, training), engagement and performance scores, and training records (programme, type, outcome, duration, cost).

## Tools

- Power BI Desktop
- Power Query (M) for cleaning and transformation
- DAX for measures and the date model
- Power BI AI visuals: Key Influencers, Decomposition Tree and Smart Narrative

## Data preparation

All transformations were done in Power Query:

1. Loaded the Excel workbook and promoted the first row to headers
2. Set explicit data types for all 28 source columns
3. Removed fully blank rows
4. Trimmed whitespace from Employee ID and converted it to a whole number
5. Renamed inconsistent columns (`Training Date` to `TrainingDate`, `Training Duration(Days)` to `TrainingDurationDays`)

Four analytical fields were added in the model:

| Derived field | Logic |
| --- | --- |
| Age Group | Under 25, 25–34, 35–44, 45–54, 55+ |
| Engagement Category | Low, Medium or High from the engagement score |
| Satisfaction Category | Low, Medium or High from the satisfaction score |
| Training Success | Successful if the outcome is Completed or Passed; Unsuccessful if Failed or Incomplete |

## Data model and DAX

- A calculated **Date table** covers 2018-08-07 to 2023-08-06 and is marked as the official date table.
- **StartDate** holds the active relationship to the Date table, which drives headcount and hiring trends.
- **Survey Date** and **TrainingDate** have inactive relationships. Specific measures switch them on with `USERELATIONSHIP`, so engagement and training trends resolve correctly without clashing with the hiring timeline.

Key measures:

| Measure | Logic |
| --- | --- |
| Total Employees | `COUNTROWS` of the employee table |
| Active / Terminated Employees | `CALCULATE(COUNTROWS(...), EmployeeStatus = ...)` |
| Attrition Rate | `DIVIDE([Terminated Employees], [Total Employees], 0)` |
| Average Engagement / Satisfaction / Work-Life Balance / Rating | `AVERAGE` of each score |
| Total / Average Training Cost | `SUM` / `AVERAGE` of training cost |
| Training Success Rate | `DIVIDE(successful records, total records, 0)` |
| Training Cost by Training Date | `CALCULATE(..., USERELATIONSHIP(...))` |

## Dashboard pages

1. **Executive Overview:** headline KPIs and the hiring trend
2. **Workforce and Diversity:** department, division, age, gender and pay-zone breakdowns
3. **Attrition Analysis:** attrition by department, state, age, engagement and satisfaction
4. **Engagement, Performance and Training:** score distributions and training outcomes
5. **AI Insights and Recommendations:** Key Influencers, Decomposition Tree, Smart Narrative and written recommendations

## Key findings

| Area | Finding |
| --- | --- |
| Overall attrition | 13.6% (387 of 2,845 employees) |
| Largest risk segment | Low-engagement Production employees: 833 people at 17.4% attrition |
| Highest-attrition department | Software Engineering at 17.9%, just ahead of Production at 16.9% |
| Performance | Employees on a Performance Improvement Plan have the highest attrition (20.9%) |
| Possible regretted attrition | Leavers had a higher average rating (3.07) than current employees (2.96) |
| Satisfaction | High-satisfaction employees showed the highest attrition (15.0%), so satisfaction alone is not a reliable predictor here |
| Training type | Internal programmes beat External on success rate (52.5% vs 49.2%) on near-identical spend of about $0.8M each |
| Training programmes | Communication Skills was strongest (54.3% success); Technical Skills weakest (45.1%); Project Management was high-cost and low-success ($329K at 47.7%) |
| Workforce mix | Production holds 67.1% of employees and Massachusetts 88.7% |

All figures are associations within this dataset, not proof of cause.

## Recommendations

1. **Run a targeted engagement programme** for low-engagement Production employees, the largest risk pool by volume, using manager check-ins and pulse surveys.
2. **Hold stay interviews in Software Engineering** to find department-specific reasons people leave.
3. **Run exit interviews with higher-rated leavers** to test for regretted attrition.
4. **Audit External training vendors and the Project Management programme**, and move budget towards Communication Skills.
5. **Review pay and benefits equity in Pay Zones A and B**, where attrition (about 14.3%) runs above Zone C (12.0%).
6. **Investigate the high-satisfaction attrition pattern** through exit interviews or a follow-up survey before relying on satisfaction scores for retention.

## Limitations

- No exit date or exit reason, so attrition can be measured as a status but not trended over time or explained.
- No salary field; Pay Zone gives only a categorical view of pay.
- No recruitment source, promotion history, absence or overtime data.
- 88.7% of employees are in one state, so findings for other states rest on very small samples.

## Next steps

- Build an attrition prediction model using engagement, satisfaction, performance and demographic fields
- Extend training analysis into ROI by linking it to post-training performance
- Add exit-reason, salary and promotion data if available, to move from correlation towards cause

## Repository contents

| File | Description |
| --- | --- |
| `README.md` | This overview |
| `HR_Analytics_Report.pdf` | Full project report with every figure, table and question answered |
| `HR_Analytics_Dashboard.pbix` | Power BI dashboard file; open it in Power BI Desktop |

## Author

**Aditi Agarwal**
PGDM (Big Data Analytics), FORE School of Management
[LinkedIn](https://www.linkedin.com/in/aditi-agarwal-323174280) · [GitHub](https://github.com/aditiagarwal2410)
