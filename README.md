🏥 Healthcare Cost & Utilization Analysis Using PostgreSQL
Executive Summary
This project explores patient encounter and cost data from a synthetic healthcare database using PostgreSQL. The goal was to uncover cost trends, utilization patterns, and payer distributions to guide data-driven decisions in healthcare management. Through SQL-based analysis and data visualization, we identified cost disparities across cities, patient demographics, and healthcare organizations.

Business Problem
Healthcare providers face growing challenges in understanding the drivers behind rising costs and uneven resource utilization. This analysis aims to answer critical questions such as:

Which cities have the highest healthcare costs?
What encounter types contribute most to total spending?
How do payer coverages and procedure costs vary across regions?
What demographic or operational patterns can help optimize costs and coverage?
Methodology
Database Setup: Four relational tables were created — patients, encounters, procedures, and payers.
Data Cleaning: Missing and null values were handled, focusing on valid encounter and cost data.
SQL Analysis: Each insight was derived using carefully structured SQL queries.
Visualization: Results were exported and visualized using Python’s Matplotlib and Seaborn libraries.
Skills Demonstrated
SQL (Joins, Aggregations, Subqueries, Grouping, Window Functions)
Data Visualization (Matplotlib, Seaborn)
Data Cleaning and Transformation
Analytical Storytelling and Insight Communication
Healthcare Data Analysis
Results & Insights
🩺 Insight 1: Average Encounter Cost by City
SELECT p.CITY,
       ROUND(AVG(e.TOTAL_CLAIM_COST), 2) AS avg_claim_cost
FROM encounters e
JOIN patients p ON e.PATIENT = p.Id
GROUP BY p.CITY
ORDER BY avg_claim_cost DESC;
Observation: Cities with higher average claim costs indicate potential inefficiencies or higher service charges. This insight highlights areas for cost-control interventions.

Visualization: Average Encounter Cost by City

💳 Insight 2: Payer Coverage vs Total Claim Cost
SELECT p.CITY,
       ROUND(AVG(e.PAYER_COVERAGE), 2) AS avg_coverage,
       ROUND(AVG(e.TOTAL_CLAIM_COST), 2) AS avg_claim
FROM encounters e
JOIN patients p ON e.PATIENT = p.Id
GROUP BY p.CITY
ORDER BY avg_claim DESC;
Observation: Some cities show a gap between claim costs and payer coverage, signaling possible under-insurance or delayed reimbursement patterns.

Visualization: Payer Coverage vs Total Claim Cost

🧾 Insight 3: Encounter Class Distribution
SELECT ENCOUNTERCLASS,
       COUNT(*) AS total_encounters,
       ROUND(AVG(TOTAL_CLAIM_COST), 2) AS avg_cost
FROM encounters
GROUP BY ENCOUNTERCLASS
ORDER BY total_encounters DESC;
Observation: Outpatient and emergency encounters account for the largest volume, yet inpatient visits remain the most costly per encounter — a common trend in healthcare systems.

Visualization: Encounter Class Distribution

🏢 Insight 4: Cost Variation by Organization
SELECT ORGANIZATION,
       ROUND(AVG(TOTAL_CLAIM_COST), 2) AS avg_claim_cost,
       ROUND(SUM(TOTAL_CLAIM_COST), 2) AS total_claim_cost
FROM encounters
GROUP BY ORGANIZATION
ORDER BY avg_claim_cost DESC;
Observation: Some organizations consistently report higher average claim costs, which could be linked to specialized services or inefficiencies. Benchmarking costs per organization helps identify outliers.

Visualization: Cost Variation by Organization

🧬 Insight 5: Most Common Procedures by City
SELECT p.CITY,
       pr.DESCRIPTION,
       COUNT(pr.CODE) AS procedure_count,
       ROUND(AVG(pr.BASE_COST), 2) AS avg_procedure_cost
FROM procedures pr
JOIN patients p ON pr.PATIENT = p.Id
GROUP BY p.CITY, pr.DESCRIPTION
ORDER BY procedure_count DESC
LIMIT 10;
Observation: The most frequent procedures often relate to general consultations and diagnostics, but their cost variance across cities may reflect regional healthcare infrastructure differences.

Visualization: Most Common Procedures by City

Recommendations
Cost Optimization: Standardize billing and negotiate better payer coverage rates for high-cost regions.
Data-Driven Resource Allocation: Prioritize infrastructure investments in cities with high procedure counts but low coverage.
Payer Collaboration: Enhance insurance claim efficiency to minimize out-of-pocket patient expenses.
Further Research: Incorporate patient outcome data to assess whether higher costs translate to better care quality.
Next Steps
Integrate additional data sources (pharmacy, lab tests) to extend insights.
Build a dashboard (Power BI or Tableau) for real-time healthcare monitoring.
Apply predictive analytics to forecast costs and identify high-risk patients.
Author: Abu Ammar
Tools Used: PostgreSQL, Python (Pandas, Matplotlib, Seaborn), Excel
Dataset Type: Synthetic Healthcare Dataset

