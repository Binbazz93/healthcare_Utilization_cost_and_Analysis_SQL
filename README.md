# 🏥 Healthcare Cost & Utilization Analysis Using PostgreSQL

## **Executive Summary**
This project explores patient encounter and cost data from a synthetic healthcare database using **PostgreSQL**.  
The goal was to uncover **cost trends**, **utilization patterns**, and **payer distributions** to guide data-driven decisions in healthcare management.  
Through SQL-based analysis and visualization, key insights were uncovered on cost disparities across cities, patient demographics, and healthcare organizations.

---

## **Business Problem**
Healthcare providers face challenges in understanding the factors driving **rising costs** and **uneven resource utilization**.  
This analysis addresses key questions such as:
- Which cities have the highest healthcare costs?
- What encounter types contribute most to total spending?
- How do payer coverages and procedure costs vary across regions?
- What demographic or operational factors can help optimize costs and coverage?

---

## **Methodology**
1. **Database Setup:** Four relational tables were created — `patients`, `encounters`, `procedures`, and `payers`.  
2. **Data Cleaning:** Missing and null values were handled, focusing on valid encounter and cost data.  
3. **SQL Analysis:** Each insight was derived using structured SQL queries.  
4. **Visualization:** Results were exported and visualized using Python libraries (**Matplotlib** and **Seaborn**).

---

## **Skills Demonstrated**
- SQL (Joins, Aggregations, Subqueries, Grouping, Window Functions)
- Data Visualization (Matplotlib, Seaborn)
- Data Cleaning and Transformation
- Analytical Storytelling and Communication
- Healthcare Cost and Utilization Analysis

---

## **Results & Insights**

### 🩺 Insight 1: Average Encounter Cost by City
```sql
SELECT p.CITY,
       ROUND(AVG(e.TOTAL_CLAIM_COST), 2) AS avg_claim_cost
FROM encounters e
JOIN patients p ON e.PATIENT = p.Id
GROUP BY p.CITY
ORDER BY avg_claim_cost DESC;
-
##**Insight 2: Payer Coverage vs Total Claim Cost
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
