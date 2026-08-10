# Healthcare Revenue Cycle & Claims Denial Dashboard

## Dashboard Features
Executive RCM KPIs: Dynamic cards tracking Total Billed, Revenue Leakage, and Denial Rates.

AI-Powered Key Influencers: Utilizes Power BI's ML capabilities to determine the statistical likelihood of claim denials based on specific reason codes.

Denial Pareto Analysis: Clustered bar charts isolating the administrative errors driving the highest financial loss.

Provider Performance Matrix: Granular drill-downs allowing management to audit revenue leakage at the individual Provider ID level.

## Repository Structure
Healthcare_RCM_Dashboard.pbix — Interactive Power BI Dashboard file.

data/claim_data.csv — Raw medical claims dataset.

images/dashboard_screenshot.png — High-resolution preview of the dashboard.

## Executive Summary
This Power BI project provides a comprehensive Revenue Cycle Management (RCM) analysis for a simulated healthcare network. By processing and modeling medical claims data, this dashboard isolates the financial impact of denied claims, identifies the root causes of revenue leakage, and provides actionable intelligence to optimize billing workflows.

---

## Key Business Insights

* **Financial Hemorrhage:** Identified a critical **32.4% revenue leakage**, with only $200K collected against $297K in total billed services.
* **The Denial Bottleneck:** **33.1%** of all claims resulted in a definitive "Denied" outcome, drastically inflating Accounts Receivable (AR) cycles.
* **Root Cause Identification:** The primary drivers of financial loss were non-medical, administrative errors. **"Incorrect billing information"** and **"Patient eligibility issues"** accounted for the vast majority of denied claims, indicating a need for front-end staff retraining.

---

## Technical Implementation

### Data Pipeline & Modeling
* **Power Query ETL:** Formatted service dates, cleansed categorical reason codes, and standardized currency formatting.
* **Data Architecture:** Transformed a flat `.csv` structure into an enterprise-grade relational **Star Schema**, creating dedicated Dimension tables for `Providers`, `Patients`, and `Denial Reasons`.

### Explicit DAX Measures
```dax
// Total Revenue Leakage
Revenue Leakage = SUM('Fact_Claims'[Billed Amount]) - SUM('Fact_Claims'[Paid Amount])

// Collection Rate
Collection Rate % = DIVIDE(SUM('Fact_Claims'[Paid Amount]), SUM('Fact_Claims'[Billed Amount]), 0)

// Denied Claims Count
Denied Claims = CALCULATE(COUNT('Fact_Claims'[Claim ID]), 'Fact_Claims'[Outcome] = "Denied")

// Claim Denial Rate
Denial Rate % = DIVIDE([Denied Claims], COUNT('Fact_Claims'[Claim ID]), 0)
