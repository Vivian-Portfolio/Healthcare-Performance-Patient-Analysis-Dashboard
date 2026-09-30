# Healthcare Performance & Patient Analytics Dashboard
> *A Power BI dashboard that tracks patient volume, hospital operations, financial performance and patient experience across Nigerian states, so leadership can see where the organisation is meeting its targets and where it is not.*

---

## ⚙️ Project Type Flags
> *Check what applies. This helps reviewers and collaborators understand the nature of the work at a glance. Delete this block before publishing.*

- [ ] Exploratory Data Analysis (EDA)
- [ ] SQL Analysis / Querying
- [x] Dashboard / Data Visualization
- [ ] Data Pipeline / ETL
- [ ] Predictive Modelling / Machine Learning
- [x] Data Cleaning / Wrangling
- [ ] End-to-End (multiple of the above)
- [ ] Other: ___________

---

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#7-analysis--metrics)
8. [Key Insights](#8-key-insights)
9. [Recommendations](#9-recommendations)
10. [Assumptions & Limitations](#10-assumptions--limitations)
11. [Future Enhancements](#11-future-enhancements)
12. [Deliverables](#12-deliverables)
13. [Author](#13-author)

---


**Context:** A healthcare organisation operating across several Nigerian states needs one place to monitor patient activity, hospital performance and finances against set targets.

**Problem Statement:** How are patient volumes, operations, revenue and patient satisfaction performing by state and over time, and where are the gaps against target?

**Approach:** I cleaned and modelled the patient visit data in Power BI using a star schema, then built DAX measures and a five-page interactive dashboard.

**Outcome:** An executive-ready dashboard with actionable insights on performance, cost, and patient experience.

---

## 2. Objectives

- **Primary Objective:** Deliver a Power BI dashboard that gives leadership a clear view of healthcare performance and patient analytics across states.
- **Secondary Objective 1:** Compare actual performance with state targets.
- **Secondary Objective 2:**  Analyse patient demographics, hospital operations, and financial performance.
- **Secondary Objective 3:**  Measure patient experience and identify areas to improve.

> 💡 *Every analysis decision in this project traces back to one of these objectives.*

---

## 3. Project Scope & Tools

| Dimension | Details |
|-----------|---------|
| **In Scope** | Patient visits, state targets, and date data from IFEXA_HealthCare_BI_Project_Dataset.xlsx |
| **Out of Scope** | External data sources and real-time/live data feeds (dataset is a static extract) |
| **Time Period** | January - December 2025 |
| **Granularity** | Row-level patient visits, aggregated by state and date |

### Tools & Technologies

| Category | Tool(s) Used |
|----------|-------------|
| Data Source | Excel (.xlsx) |
| Data Cleaning & Transformation |Power Query (Power BI)|
| Modelling & Calculations| Dax (Dats Analysis Expressions) - 18  custom measures |
| Visualization | Power BI Desktop |
| Version Control | Git & GitHub |

---

## 4. Repository Structure

```
├── README.md
├── data/
│   ├── raw/          # Original dataset
│   └── cleaned/      # Cleaned dataset
├── dashboard/
│   └── healthcare_dashboard.pbix
├── screenshots/      # Dashboard page images
└── presentation/     # 3–5 minute presentation
```

---

## 5. Data Workflow

1. **Source:** IFEXA_HealthCare_BI_Project_Dataset.xlsx, provided as a single Excel workbook with 3 sheets: Patient_Visits (1,200 rows), State_Targets (4 rows), Date_Table (365 days, 2025 only)."
2. **Ingestion:** Loaded into Power BI via Get Data → Excel. All three sheets imported as separate queries into Power Query."
3. **Cleaning:** No nulls or duplicate rows found in Patient_Visits. Trimmed and cleaned all text columns. Fixed a data-type issue where Visit_Date and Date columns loaded as serial numbers rather than dates — corrected via column type conversion. Fixed a spelling error in the Patient_Type conditional column ('Returing' → 'Returning') via Replace Values."
4. Transformation:** Created 3 new columns via conditional logic: Age_Group (6 bands: 0-12 through 66+), Patient_Type (New vs Returning, based on Visit_Count > 1), and Satisfaction_Band (5 bands: 0-1 through 4-5, based on Satisfaction_Score). Built a star schema with Fact_Visits (renamed from Patient_Visits) as the central table, connected to Dim_Date, Dim_State, Dim_Branch, and Dim_Department via one-to-many relationships. Marked Dim_Date as the official date table."
5. **Analysis:** 18 DAX measures created, covering totals (Revenue, Cost, Profit, Patients, Visits), rates (Profit Margin %, Achievement %, Returning %), time intelligence (MoM/YoY growth), and averages (Waiting Time, Satisfaction, Revenue per Patient). Cross-analyzed by State, Branch, Department, Age Group, Gender, Diagnosis, and Outcome across 5 report pages plus 1 drill-through detail page."
6. **Output:** Power BI file (.pbix), cleaned dataset export, GitHub repository with README and dashboard screenshots, 3-5 minute presentation."

---

## 6. Data Model & Schema

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `Patient_ID` | string  | Unique patient identifier | PT-0008  |
| `Visit_Date` | date | Date the visit was recorded| 2025-03-14 | 
| `State` | string | Nigerian states where the visit took place | Lagos |
| `Branch` | string |Hospital branch | 2025-03-14 | Lekki |
| `Department` | string | Hospital department | 2025-03-14 |Laboratory|
| `Service` | string |Specific service provided | Laboratory Test|
| `Diagnosis` | string |Recorded diagnosis| Malaria |
| `Age` |int | Patient age|  24 |
| `Gender` | string |Male / Female |Female| 
| `Visit_Count` | int |Total visits by this patient (1-4) | 3 |
| `Waiting_Time_Min` | int| Minutes waited |45 
| `Satisfaction_Score` | float |Patient satisfaction, 1-5 |4.20 |
| `Revenue_NGN` | float |Revenue for the visit |29890.17 |
| `Cost_NGN` |float| Cost for the visit| 23000 |
|`Outcome` | string | Recovered / Follow-up / Admitted / Referred | Follow-up |
|`Payment_Method`| string | Cash / Card / Transfer | Cash |
|`Insurance_Type` | string |Insurance category | Private HMO |
|`Age_Group` | string | Derived: 0-12 to 66+ | 18-35 |
|`Patient_Type` | string | Derived: New / Returning  | Returning |
|`Satisfaction_Band`| string | Derived: 0-1 to 4-5 | 3-4

Row count: 1,200 patients (2,920 total visits)
Date range:
> **Row count (approx.):1,200 patients (2,920 total visits)
> **Date range:**  2025-01-01 – 2025-12-31

## Dataset / Table: Dim_State (from State_Targets sheet

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
|`State`| string | Nigerian state | Anambra |
|`Annual_Revenue_Target_NGN`| float | ₦50,000,000 |
> ** Row count: 4 states

## Dataset / Table: Dim_Date (from Date_Table sheet

| Field Name  | Data Type | Description  | Example Value |
|-------------|-----------|--------------|---------------|
|`Date` |date | Calendar date | 2025-01-01 |
|`Year`| int | Calendar year | 2025 |
|`Month_Number`| int | Month as number | 1 |
|`Month` | string | Month name | January |
|`Quarter`| string | Quarter | Q1 |

> **Row count: 365 days (Jan 1 – Dec 31, 2025)
Marked as the official Date table in Power BI's model settings.

---

## 8. Analysis & Metrics

### Key Metrics Defined

| Metric | Plain-Language Definition | Why It Matters |
|--------|--------------------------|----------------|
| `Total Patient]` | Distinct count of patients treated across all branches in 2025 (1,200) |Establishes the size of the patient base for all other ratio |
| `Total Visits` | Sums all visit, including repeat visits (2,920)|  Shows true care volume - distinct from patient count, since each patient can visit up to 4 times  |
| `Target Revenue` | Sum of revenue across all visits (₦47.84M)  | Core financial pout put measure |
| `Target Achievement %` | Actual revenue / pro -rated annual target by state | Flags whether revenue targets are realistic and where performance gaps exist |
| `Avg Satisfaction Score]` | Mean satisfaction scores (1-5) across  all visit (3.71) |Tracks patients experience quality independent of financial performance  |
| `Returning Patients %` | share of patients with more than one visits (74%) | Indicates patients retention/ loyalty to the health system  |
| `Profit Margin %` | [(Revenue - Cost ) / Revenue (~38%) | Shows financial substainability per visit |

### Methods Used
Methods Used
- Descriptive statistics — distribution of age, revenue, waiting time, satisfaction; outlier and data-quality checks
- Trend analysis of monthly patient visits and revenue across 2025
- Segmentation/group comparison by department, branch, state, age group, and patient type (new vs. returning)
- Correlation analysis between waiting time and satisfaction score (scatter plot + trend line)
- Custom DAX measures in Power BI for target pro-ration, YoY/MoM growth, and patient retention funnel logic

---

## 9. Key Insights

**Insight 1: Revenue targets are set unrealistically high**
Actual 2025 revenue reached only ~24% of the combined state targets (₦47.84M vs. ₦200M). Even the best-performing state, Abuja, hit just ~27–28%, while Anambra lagged at ~21%. This gap is consistent across every state, which suggests the targets themselves — not underperformance — are the issue, and should be reviewed rather than treated as a KPI failure.

**Insight 2: Waiting time has no meaningful effect on patient satisfaction**
A scatter plot of waiting time vs. satisfaction score, with a trend line, shows an essentially flat relationship. Pharmacy has both short wait times and the highest satisfaction, while Outpatient has short waits but comparatively lower satisfaction — meaning other factors (likely staff interaction, communication, or diagnosis outcome) drive satisfaction more than speed of service.

**Insight 3: Departments are operationally similar but differ in patient outcomes**
Waiting time (43–47 min) and satisfaction (3.60–3.74) are nearly uniform across all departments. However, outcome mix varies: Pharmacy has the lowest recovery rate (50.5%) and highest follow-up rate (23.7%), while Laboratory has the highest recovery rate (58.8%). This points to differences in case complexity or treatment pathway rather than service quality.

**Insight 4: Most patients are returning, not new**
889 of 1,200 patients (74%) are returning patients, averaging 2.43 visits each. This signals either strong patient trust/retention or a pattern of chronic/recurring conditions requiring multiple visits — worth investigating further to know which.

**Insight 5: Data quality issues exist and should be disclosed, not silently corrected**
94 male patients are recorded under the Maternity department, and 183 of 221 Pediatrics patients are aged 18+. Rather than deleting or "fixing" these records (which would distort the dataset), they are flagged here as likely data entry or categorization errors for the client to investigate at the source.

---

## 10. Recommendations

| Priority  | Recommendation | Based On  | Suggested Owner | 
------------|----------------|-----------|-----------------|
| High | Review and reset state-level revenue targets using historical actuals rather than fixed annual figures | Insight 1 | Finance / Executive team
| High| Investigate root cause of low Pharmacy recovery/high follow-up rate — compare treatment protocols against Laboratory | Insight 3| Clinical Operations
| High | Audit Maternity and Pediatrics records for gender/age miscategorization at the point of data entry | Insight 5 | Health Records / IT
| Medium |  Shift patient experience initiatives away from wait-time reduction toward staff communication/service quality, since wait time doesn't drive satisfaction | Insight 2 | Patient Experience team
| Medium | Build a retention/loyalty program targeting the 74% returning patient base to understand and reinforce what's driving repeat visits| Insight 4 || Marketing / Patient Relations
| Low |  | Expand target-setting to branch and monthly granularity instead of annual/state-only, to enable more precise tracking | Insight | Finance

---

## 11. Assumptions & Limitations

### Assumptions
- Each row in Fact_Visits represents one patient's cumulative record for the year, with Visit_Count representing total visits — not one row per individual visit event.
- Revenue targets, provided only at annual/state grain, were assumed to scale evenly across months for the Revenue Target measure (annual target ÷ 12 × months in filter context).
- Records with data quality issues (e.g., male patients in Maternity) were treated as genuine entries for analysis purposes rather than excluded, since there was no way to verify or correct the true value.

### Limitations
- Revenue targets cannot be broken down by branch or department, only by state and year — limiting how precisely underperformance can be localized.
- The dataset covers only 2025, so no year-over-year trend analysis is possible.
- No qualitative data (e.g., patient comments, staff notes) exists to explain why satisfaction or outcomes vary — the dashboard identifies patterns but not root causes.
- Known data quality issues (Maternity/Pediatrics mismatches) were not corrected, so downstream department-level figures for those two departments should be read with that caveat in mind.
*A skeptic might ask: "If some Maternity/Pediatrics records are wrong, can we trust any department-level numbers?" — Yes, for all other departments; the flagged issues are isolated to two departments and were identified precisely because they were checked, not assumed correct.*

---

## 12. Future Enhancements
- [ ] Add branch- and month-level revenue targets to enable more granular achievement tracking
- [ ] Build a patient-level drill-through page (by Patient_ID) for deeper case investigation
- [ ] Incorporate multi-year data once available, to enable real YoY trend analysis
- [ ] Add a data-quality monitoring page that flags mismatches (e.g., gender/department, age/department) automatically as new data loads


---

## 13. Deliverables

| Deliverable | Description | Location |
|-------------|-------------|----------|
| Power BI file (.pbix) | Full interactive dashboard — 5 report pages, drill-through, tooltips, bookmarks | `Health Care.pbix` |
| Cleaned dataset |Excel/CSV export of the transformed dataset (with Age_Group, Patient_Type, Satisfaction_Band columns added | `IFEXA_HealthCare_BI_Project_Dataset.xlsx` |
| GitHub repository] | Contains .pbix file, cleaned dataset, screenshots folder, and this README | `github.com/Vivian-Portfolio` |
| Dashboard screenshots | Image of each of the 5 report pages + Branch Detail drill-through page | [`/path/to/file`] |
| Presentation | 3–5 minute walkthrough covering problem, analysis, dashboard demo, findings, and recommendations | [`/path/to/file`] |

---

## 14. Author

**Vivian Okwara**
Data Analyst | Lagos, Nigeria 

- 🔗 LinkedIn: https://linkedin.com/in/okwara-vivian
- 💼 https://Vivian-Portfolio. github.io
- 📧 Email: okwaravivian26@gmail.com
---

*Last updated: [Month YYYY]*
*If this template helped you, consider starring the repository.*
