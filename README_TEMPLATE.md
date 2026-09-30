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

Scope
Dimension
Details
In Scope
Patient visits, state targets, and date data from IFEXA_HealthCare_BI_Project_Dataset.xlsx
Out of Scope

Time Period
[Insert date range from the Date Table]
Granularity
Row-level patient visits, aggregated by state and date
Tools & Technologies
Category
Tool(s) Used
Data Source
Excel (.xlsx)
Data Cleaning & Transformation
Power Query (Power BI)
Modelling & Calculations
Power BI, DAX
Visualisation
Power BI Desktop
Version Control
Git & GitHub

-->

| Dimension | Details |
|-----------|---------|
| **In Scope** | Patient visits, state targets, and date data from IFEXA_HealthCare_BI_Project_Dataset.xlsx |
| **Out of Scope** | External data sources and real-time/live data feeds (dataset is a static extract) |
| **Time Period** | [Insert date range from the Date Table] |
| **Granularity** | Row-level patient visits, aggregated by state and date |

### Tools & Technologies

| Category | Tool(s) Used |
|----------|-------------|
| Data Source | Excel (.xlsx) |
| Data Cleaning & Transformation |Power Query (Power BI)|
| Modelling & Calculations| [e.g., pandas, dplyr, custom SQL queries] |
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

## Dataset / Table: Dim_State (from State_Targets sheet)
| Field Name | Data Type | Description | Example Value |
|`State`| string | Nigerian state | Anambra |
|`Annual_Revenue_Target_NGN`| float | 50,000,000 |
> ** Row count: 4 states

## Dataset / Table: Dim_Date (from Date_Table sheet)**
| Field Name  | Data Type | Description  |Example Value |
|`Date` |date | Calendar date | 2025-01-01 |
|`Year`| int | Calendar year | 2025 |
|`Month_Number`| int | Month as number | 1 |
|`Month` | string | Month name | January |
|`Quarter`| string | Quarter | Q1 |

>**Row count: 365 days (Jan 1 – Dec 31, 2025)
Marked as the official Date table in Power BI's model settings.

---

## 8. Analysis & Metrics

### Key Metrics Defined

| Metric | Plain-Language Definition | Why It Matters |
|--------|--------------------------|----------------|
| `[Metric 1]` | [What it measures, in one sentence] | [What decision or question it answers] |
| `[Metric 2]` | [What it measures, in one sentence] | [What decision or question it answers] |
| `[Metric 3]` | [What it measures, in one sentence] | [What decision or question it answers] |

### Methods Used

- [e.g., Descriptive statistics - distribution, central tendency, outlier detection]
- [e.g., Trend analysis across [time period]]
- [e.g., Segmentation / group comparison by [dimension]]
- [e.g., Correlation analysis between [variable A] and [variable B]]
- [e.g., SQL window functions for [specific aggregation]]
- [e.g., Custom aggregation or transformation logic in [tool]]

---

## 9. Key Insights

**Insight 1: [Short descriptive headline]**
[What you found + what it suggests. One short paragraph.]

**Insight 2: [Short descriptive headline]**
[What you found + what it suggests.]

**Insight 3: [Short descriptive headline]**
[What you found + what it suggests.]

**Insight 4 (if applicable): [Short descriptive headline]**
[What you found + what it suggests.]

---

## 10. Recommendations


| Priority | Recommendation | Based On | Suggested Owner |
|----------|---------------|----------|-----------------|
| High | [Specific, actionable step] | [Insight it comes from] | [Who should act] |
| Medium | [Specific, actionable step] | [Insight it comes from] | [Who should act] |
| Low | [Exploratory or longer-term suggestion] | [Insight it comes from] | [Who should act] |

---

## 11. Assumptions & Limitations


### Assumptions
- [What did you treat as true without being able to verify?]
- [What simplifications did you make for scope or feasibility?]
- [What domain rules or definitions did you accept as given?]

### Limitations
- [What gaps exist in the data?]
- [What analysis was out of scope but could affect interpretation?]
- [What would a more rigorous version of this project include?]
- [Are there known biases in the data source or collection method?]

> *The goal here is pre-emptive Q&A. What would a thoughtful skeptic push back on? Document the answer here, before they ask.*

---

## 12. Future Enhancements

<!--
  WHAT GOOD LOOKS LIKE:
  ✅ "Automate the monthly data pull from the POS export folder using
      a scheduled Python script, replacing the current manual process."
  ✅ "Expand the return rate analysis to include carrier-level data,
      which was unavailable in this dataset but exists in the logistics system."

  WHAT TO AVOID:
  ❌ "Add a machine learning model."
     (Vague, and disconnected from the actual findings of this project.)
  ❌ Listing aspirational features that don't follow logically from the work.
-->

- [ ] [Enhancement 1 - specific and traceable to a real gap in this project]
- [ ] [Enhancement 2]
- [ ] [Enhancement 3]
- [ ] [Enhancement 4]

---

## 13. Deliverables

| Deliverable | Description | Location |
|-------------|-------------|----------|
| [Name] | [What it contains] | [`/path/to/file`] |
| [Name] | [What it contains] | [`/path/to/file`] |
| [Name] | [What it contains] | [`/path/to/file`] |

---

## 14. Author

**[Your Name]**
[Your role or title - current or target]

- 🔗 [LinkedIn URL]
- 💼 [Portfolio or GitHub profile URL]
- 📧 [Email - optional]

---

*Last updated: [Month YYYY]*
*If this template helped you, consider starring the repository.*
