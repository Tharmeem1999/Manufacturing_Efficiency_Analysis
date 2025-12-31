# 🥤 Manufacturing Efficiency Analysis (Wolf Cola)

![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-yellow)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📄 Executive Summary
**Goal:** Analyze manufacturing line data to identify root causes of low productivity and propose actionable solutions to increase efficiency.

**Key Result:** Identified that **56% of downtime is caused by Operator Error**, specifically around "Machine Adjustments." Proposed a targeted training program and Standard Operating Procedure (SOP) implementation projected to recover **~5 hours of production time per week**.

---

## 💼 Business Problem
**The Client:** Wolf Cola, a soft drink manufacturer based in Philadelphia.
**The Challenge:** The bottling production line is experiencing inconsistent output and unexplained downtime. The previous manager left raw data files but no analysis.
**The Objective:**
1.  Calculate the overall **Line Efficiency %**.
2.  Identify the top **Downtime Factors** hindering production.
3.  Analyze **Operator Performance** to find training gaps.

---

## 🛠️ Data Transformation & Modeling
The raw data presented a "Wide" format challenge that required significant transformation before analysis could begin.

### 1. Data Cleaning (Power Query)
* **Unpivoting Data:** The `Line Downtime` table was originally in a wide format (12 columns for downtime reasons). I performed an **Unpivot** operation to convert it into a "Tall" format (Attribute/Value), enabling proper relationships and aggregation.
* **Time Calculation:** Created a custom column `Actual Duration (Mins)` by subtracting `Start Time` from `End Time` and converting the duration to total minutes.
* **Data Typing:** Fixed data type issues where Duration was reading as "Days" instead of "Minutes."

### 2. Data Modeling (Star Schema)
Built a relational data model to connect transaction tables with dimension tables:
* **Fact Tables:** `Line Productivity`, `Line Downtime`
* **Dimension Tables:** `Products`, `Downtime Factors`
* **Relationships:** One-to-Many relationships established between Products/Factors and the Fact tables.

![Data Model Screenshot](Add_Your_Model_View_Screenshot_Here.png)
*(Note: Replace this placeholder with a screenshot of your Model View)*

---

## 📊 Key Insights
The analysis of **38 production batches** revealed the following:

### 1. Operational Efficiency
* **Overall Line Efficiency:** `64.0%` (Target: 85%+)
* **Total Downtime:** `1,388 Minutes` (approx. 23 hours of lost time)

### 2. Top Downtime Causes
The data shows that internal factors are the primary bottleneck, not machine failure.
* **#1 Cause:** Machine Adjustment (332 mins)
* **#2 Cause:** Machine Failure (254 mins)
* **#3 Cause:** Inventory Shortage (225 mins)

### 3. Operator Performance
* **Operator Error Rate:** `55.9%` of all downtime is attributed to human error.
* **Top Performer:** **Charlie** (66.8% Efficiency)
* **Lowest Performer:** **Mac** (60.9% Efficiency)

![Dashboard Screenshot](Add_Your_Dashboard_Screenshot_Here.png)
*(Note: Replace this placeholder with a screenshot of your main dashboard)*

---

## 🚀 Recommendations
Based on the data, I recommend the following immediate actions:

1.  **Implement "Machine Adjustment" SOPs:** Since 332 minutes were lost to adjustments (the top downtime factor), create a standardized checklist for operators to follow. This will reduce variability and "trial and error" time.
2.  **Operator Shadowing Program:** Pair **Mac** (Lowest Efficiency) with **Charlie** (Highest Efficiency) for a one-week shadowing session to transfer best practices on line speed management.
3.  **Fix Inventory Staging:** 225 minutes were lost to "Inventory Shortage." This is a preventalbe systemic issue. Supply chain staff must stage materials line-side *before* the shift begins.

---

## 🧮 Technical Appendix: DAX Measures
Key calculations used in this report:

**1. Line Efficiency %**
```dax
Line Efficiency % = DIVIDE([Total Expected Time], [Total Actual Time], 0)
```

**2. Total Expected Time**
```dax
Total Expected Time = SUMX('Line Productivity', RELATED('Products'[Min batch time]))
```

**3. Total Downtime**
```dax
Total Downtime = SUM('Line Downtime'[Downtime Minutes])
```
