**School Staff Performance & Increment Framework (Power BI)**

**Problem**
Our school gives a flat 10% annual increment to every employee. Some employees are meant to get an additional performance-based increment on top of that, but there was no defined way to measure "performance" — especially across departments as different as Accounts, IT, Teaching, Security, and Maintenance (plumbers, electricians, gardeners, cleaners). A single performance metric doesn't work across that range of roles: a teacher's performance isn't measured the same way as a security guard's.
My Role
I identified the measurement gap, designed the KPI framework and role-bucket logic, built the data structure, and built the Power BI dashboard end-to-end — from raw data model to DAX measures to increment-tier recommendation.
The Data
This project uses a synthetic (sample) dataset — 22 employees across 10 departments, 12 months of activity — built to prove the framework works before it's pointed at real HR records. It is not real school data.
Departments are grouped into 3 buckets because their performance drivers differ:
1. Teaching — Regular & Visiting teachers
2. Office — HR, Accounts, Admin, IT, Media
3. Support — Domestic Staff, Security, Maintenance (plumber, electrician, gardener, painter, cleaner)
**Raw tables**: Employees, Attendance (monthly), Complaints (monthly), Task_Completion (monthly, Office/Support), Academic_Results (per term, Teaching), Weights (editable, bucket-specific KPI weights).

**Tools & Techniques**
1. **Excel** — designed the raw data structure and column schema that real campus data will be entered into; built and validated the scoring logic first with SUMIFS, AVERAGEIFS, INDEX/MATCH, and IFERROR before touching Power BI, so the model's logic was proven on a known-correct calculation before being rebuilt in DAX.
2.**Power BI / DAX** — imported the raw tables (not the pre-calculated Excel summary) and rebuilt every KPI as a DAX measure, so the dashboard is a genuine live model rather than a static Excel-to-PDF export: DIVIDE, SUMX, AVERAGEX, LOOKUPVALUE, SWITCH, FILTER, COUNTROWS.
3. **Data modeling** — star schema with Employees as the central dimension table, one-to-many relationships to each fact table on EmployeeID, and a Bucket-based lookup into the Weights table.

**Process**
1.	Mapped every department to one of 3 performance buckets, since a single KPI set can't fairly compare a teacher to a plumber.
2.	Defined 4 core KPIs per employee: Attendance Rate, Punctuality Rate, Complaint-Free Score, Output Score (Output = academic score for Teaching, on-time task completion for Office/Support).
3.	Set bucket-specific weights for each KPI (e.g., Output counts for 50% of a teacher's score but only 20% of a Support employee's score, where attendance and conduct matter more).
4.	Built a weighted Composite Score (0–100) per employee, an Increment Tier (High ≥85 / Medium ≥70 / Low <70), and a Suggested Increment % (base 10% ± tier adjustment) — a starting formula for HR to review, not a final policy.
5.	Validated the model in Excel first, then rebuilt every calculation as a DAX measure in Power BI and cross-checked the numbers matched.
6.	Built a 4-page dashboard: Overview, Bucket Drill-down, Monthly Trends, Individual Employee Detail.
**Key Insights (from the sample dataset)**
1. 6 of 22 employees (≈27%) fall into the "High" performance tier under this model.
2. The Support-staff bucket has the highest total complaint count across the year (71, vs. 44 for Office and 38 for Teaching) — despite being similar in headcount to Office — suggesting complaint volume, not just attendance, is a meaningful differentiator for that bucket's scoring.
3. HR shows the highest single department average composite score, but with only one HR employee in this sample, that's not a statistically meaningful comparison — a real dataset with more HR staff is needed before drawing conclusions at the department level.
**Business Impact**
Gives HR a transparent, auditable basis for performance-based increments instead of a subjective annual call — every score traces back to attendance, punctuality, complaint, and output records rather than manager discretion alone. The weights are editable, so HR can adjust how much each KPI matters per role bucket without changing the underlying model.
Challenges & Learnings
1. Fair comparison across very different roles was the core challenge — solved by bucketing departments and giving each bucket its own KPI weights instead of forcing one formula on everyone.
2. Small-sample distortion (like the single-HR-employee average above) is a real risk with any department-level rollup — this taught me to always report sample size alongside an average, not just the number.
3. Measure vs. calculated column in DAX: some visuals (like a donut chart legend) need a fixed per-row value, not a measure recalculated in row context — a distinction that isn't obvious until a visual silently returns blank.
4. Next step: replace the synthetic data with real attendance/complaint/task records and re-validate the weights with HR before this is used for actual increment decisions.

**File	Description**
**School_Performance_Framework.xlsx** Sample dataset (22 employees, 12 months) — raw attendance, complaints, task completion, and academic records with editable KPI weights, used to validate the scoring logic before building it in Power BI.

**Performance_summary.pbix**	Power BI dashboard — DAX-driven weighted performance scoring and increment-tier recommendation across Teaching, Office, and Support staff buckets.

**README.md** Project overview — problem, approach, KPI logic, key insights, and challenges.

**screenshots/overview.png**	Overview page — average composite score, headcount, and tier distribution across departments.

**screenshots/by_bucket.png**	Bucket drill-down — role-specific KPI table filterable by Teaching/Office/Support.

**screenshots/trends.png**	Monthly attendance and complaint trends by bucket across the year.

**screenshots/employee_detail.png**	Individual employee KPI breakdown and suggested increment.

