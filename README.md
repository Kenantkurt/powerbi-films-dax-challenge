# 🎬 Power BI – Films DAX Challenge

## Dashboard Preview
![Dashboard](screenshots/dashboard.png)


## Data Model
![Data Model](screenshots/model_view-powerbi.png)



![Power BI](https://img.shields.io/badge/Tool-Power%20BI-yellow)
![DAX](https://img.shields.io/badge/Language-DAX-blue)
![Analytics](https://img.shields.io/badge/Domain-Business%20Intelligence-success)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

This project is a Power BI analytical solution designed to evaluate movie profitability
with a strong emphasis on data modeling correctness, metric reliability,
and business logic accuracy.

Rather than focusing solely on visualization, the project prioritizes how
data structure, aggregation logic, and filter context directly impact analytical outcomes.

---

## 📊 Project Overview

The core objectives of this project were to:

- Design a robust fact–dimension data model suitable for analytical workloads
- Implement explicit, reusable DAX measures aligned with business logic
- Distinguish between revenue-weighted and unweighted profitability metrics
- Handle data quality issues upstream to avoid distorted KPIs
- Build a dashboard where filter behavior is predictable and controlled

---

## 📂 Data Model & Structure

### Tables Used

- Films (Fact table)
  - Budget
  - Box Office revenue

- Genre (Dimension table)
- Certificate (Dimension table)

### Relationships

- Genre → Films (one-to-many)
- Certificate → Films (one-to-many)
- Single-direction filtering (best practice)

This star schema ensures:

- Clear separation of facts and dimensions
- Correct filter propagation
- Stable and interpretable DAX calculations

---

## 🧠 Key DAX Measures & Logic

### Weighted Profit Margin (%)

Margin % =
VAR revenue =
    SUM ( Films[BoxOfficeDollars] )
VAR margin =
    revenue - SUM ( Films[BudgetDollars] )
RETURN
DIVIDE ( margin, revenue )

Engineering perspective:

This measure computes profitability at the aggregate level, where each movie’s contribution
is weighted by its revenue.

High-revenue movies therefore have a proportionally larger impact on the final metric,
making this calculation suitable for evaluating overall financial performance.

Business question answered:

“How profitable is this group overall, considering revenue scale?”

---

### Unweighted Average Profit Margin per Movie

Avg Margin % per Movie =
AVERAGEX (
    VALUES ( Films[Title] ),
    [Margin %]
)

Engineering perspective:

This measure evaluates profitability at the row level (per movie) and then averages the results,
ensuring that each movie contributes equally regardless of size.

The use of AVERAGEX is critical to avoid misleading results that would arise
from aggregating totals before averaging.

Business question answered:

“On average, how profitable is an individual movie in this group?”

---

## ⚠️ Handling Missing Data

During data exploration, some movies were identified with missing Budget or Box Office values.

Instead of compensating for this inside DAX logic:

- Incomplete rows were removed at the Power Query (ETL) stage

This approach ensures:

- Metrics are computed only on valid records
- DAX logic remains simple and transparent
- Analytical results are not skewed by incomplete data

---

## 📈 Dashboard Overview

The final dashboard is designed with explicit interaction control:

Genre Slicer:
- Filters the certificate slicer and the main table
- Does not affect the genre-level bar chart

Certificate Slicer:
- Filters all visuals on the page

Main Table (Per Movie):
- Total Budget (M$)
- Total Box Office (M$)
- Profit Margin (%)

Bar Chart:
- Unweighted average profit margin summarized by Genre

Filter interactions were deliberately configured to ensure analytical consistency
and avoid accidental cross-filtering.

---

## 📈 Key Learnings

- Analytical accuracy depends heavily on data model design
- Implicit aggregations can hide critical assumptions
- VAR improves both performance and readability in complex DAX
- AVERAGEX is essential for correct row-level averaging
- Data quality should be enforced before metric calculation
- Understanding filter context is fundamental to reliable BI solutions

---

## 🛠 Tools Used

- Power BI Desktop
- DAX
- Power Query

---

## 🖼 Screenshots

Dashboard and data model screenshots are available in the screenshots folder.

---

## 🏷 Topics / Hashtags

Power BI · DAX · Business Intelligence · Data Analytics · Data Modeling ·
Profitability Analysis · Analytics Engineering · Portfolio Project

