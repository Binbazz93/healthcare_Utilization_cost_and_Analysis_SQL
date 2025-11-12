## **Healthcare Cost & Utilization Analysis Using PostgreSQL**

## **Executive Summary**

This project analyzes patient encounter and cost data from a synthetic healthcare dataset using PostgreSQL.

The goal was to surface cost drivers, utilization patterns, and payer behavior to inform operational and financial decisions.

Using SQL for extraction and aggregation and visualizations for storytelling, the analysis highlights city-level cost disparities, payer coverage patterns, encounter-class volumes, organization-level cost variation, and frequent procedures by city.


## **Business Problem**

Healthcare administrators and payers need to understand:

-Which geographic areas have the highest encounter costs?

-Which payers cover the greatest share of claims?

-How encounter type and organizations drive cost and volume?

-Which procedures are most frequent and how their costs vary regionally?

This analysis helps identify where to target cost-control, payer negotiations, capacity planning, and further quality analyses.


## **Methodology**

Database design: Four relational tables — patients, encounters, procedures, and payers — were created and normalized.

Data cleaning: Nulls and invalid costs were filtered out; date fields normalized; currency rounding applied for readability.

SQL analysis: Key metrics computed using JOINs, GROUP BY, aggregate functions, and window functions where appropriate.

Visualization: Query outputs exported and plotted (PNG) for inclusion in the README and portfolio.


## **Skills Demonstrated**

-PostgreSQL: joins, aggregations, NULL-safe arithmetic, grouping, 

-Data transformation and cleaning best-practices

-Visualization and presentation of findings (PNG charts)

Business interpretation and recommendations for healthcare stakeholders


## **Results & Insight**
![Graph Visualiser Insight 1](images/graph_visualiser-Insight 1.png)

 **Insight 1 — Average Encounter Cost by City**

```SQL Query

SELECT 
    p.CITY,
    ROUND(AVG(e.TOTAL_CLAIM_COST), 2) AS avg_total_claim_cost,
    COUNT(e.ID) AS total_encounters
FROM encounters e
JOIN patients p ON e.PATIENT = p.ID
WHERE e.TOTAL_CLAIM_COST IS NOT NULL
  AND p.CITY IS NOT NULL
GROUP BY p.CITY
ORDER BY avg_total_claim_cost DESC;
```


## **Observation**

City-level averages reveal substantial variation in encounter costs. High average-cost cities may indicate specialized care centers, higher operational costs, or pricing differences — and merit deeper investigation into payer mix and case complexity.

Visualization Placeh

## **Insight 2 — Cost Coverage Ratio by Payer**

```SQL Query

SELECT 
    pay.name AS payer_name,
    ROUND(AVG(e.PAYER_COVERAGE / NULLIF(e.TOTAL_CLAIM_COST, 0)), 2) AS avg_coverage_ratio,
    COUNT(e.ID) AS total_encounters
FROM encounters e
JOIN payers pay ON e.PAYER = pay.ID
WHERE e.PAYER_COVERAGE IS NOT NULL
  AND e.TOTAL_CLAIM_COST IS NOT NULL
GROUP BY pay.name
ORDER BY avg_coverage_ratio DESC;
```

## **Observation**
Payers differ significantly in how much of total claim costs they cover on average. Payers with low average coverage increase patient out-of-pocket burden and may correlate with higher unpaid claims.

(Visualization Placeholder: PNG chart goes here)

## **Insight 3 — Encounter Class Distribution (Volume vs Cost)**

SQL Query

SELECT 
    ENCOUNTERCLASS,
    COUNT(*) AS total_encounters,
    ROUND(AVG(TOTAL_CLAIM_COST), 2) AS avg_cost
FROM encounters
GROUP BY ENCOUNTERCLASS
ORDER BY total_encounters DESC;


## **Observation**
Outpatient and emergency encounters typically account for the largest volume, while certain encounter classes (e.g., inpatient, surgical) have much higher average costs per encounter. This distinction is vital for capacity and budget planning.

(Visualization Placeholder: PNG chart goes here)

## **Insight 4 — Cost Variation by Organization**

```SQL

SELECT 
    ORGANIZATION,
    ROUND(AVG(TOTAL_CLAIM_COST), 2) AS avg_claim_cost,
    ROUND(SUM(TOTAL_CLAIM_COST), 2) AS total_claim_cost,
    COUNT(ID) AS encounters_count
FROM encounters
GROUP BY ORGANIZATION
ORDER BY avg_claim_cost DESC;
```

## **Observation**
Some organizations show consistently higher average claim costs — possibly due to specialization (e.g., tertiary care), different billing practices, or patient severity mix. These organizations are candidates for cost benchmarking and targeted reviews.

(Visualization Placeholder: PNG chart goes here)

## **Insight 5 — Most Common Procedures by City**

```SQL Query

SELECT 
    p.CITY,
    pr.DESCRIPTION,
    COUNT(pr.CODE) AS procedure_count,
    ROUND(AVG(pr.BASE_COST), 2) AS avg_procedure_cost
FROM procedures pr
JOIN patients p ON pr.PATIENT = p.ID
WHERE pr.CODE IS NOT NULL
GROUP BY p.CITY, pr.DESCRIPTION
ORDER BY procedure_count DESC
LIMIT 10;
```

## **Observation**
The most frequent procedures are often diagnostics and routine treatments. However, average procedure costs vary across cities — indicating regional price differences, technology availability, or provider-level billing policies.

(Visualization Placeholder: PNG chart goes here)

## **Recommendations**

*Negotiation & Contracts: Prioritize payer contract reviews with payers showing low average coverage ratios; target improvements in reimbursement rates and claims processing.

*Cost Standardization: Investigate high-cost cities and organizations to determine if costs reflect case-mix complexity or inconsistent billing practices; implement standardized billing guidelines where possible.

*Capacity Planning: Use encounter-class seasonality and volume data to align staffing, bed allocations, and supply procurement with observed peaks.

*Patient Financial Support: For regions with high costs and low payer coverage, design patient financial counseling and sliding-scale assistance programs.

*Follow-up Analysis: Link outcome metrics (readmission, mortality, patient satisfaction) to cost to evaluate value (cost vs. quality).

## **Next Steps**

Add outcome data (e.g., readmissions, patient outcomes) to assess the relationship between cost and quality.

Build an interactive dashboard (Power BI / Tableau) for stakeholders to filter by city, payer, organization, and time.

Perform time-series analysis (monthly/seasonal patterns) and predictive modeling for staffing and inventory forecasting.

Drill into case-mix using diagnosis and procedure codes to control for severity when benchmarking costs.

## **Project Details**

Detail Value

Author Abu Ammar

Tools PostgreSQL, Excel

Data Type Synthetic healthcare dataset (patients, encounters, procedures, payers)
