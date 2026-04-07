#  Posh Hairs Revenue Performance Analysis
*A Data-Driven Investigation into Revenue performance, gap analysis and contribution to the business finance*

---
## TABLE OF CONTENT
- [CONTEXT AND PROBLEM](#CONTEXT-AND-PROBLEM) 
- [DATASET METADATA](#DATASET-METADATA) 
- [DATA QUALITY AND CLEANING STRATEGY](#DATA-QUALITY-AND-CLEANING-STRATEGY) 
- [HYPOTHESIS EXPLORATION](#HYPOTHESIS-EXPLORATION) 
- [BUSINESS IMPACT](#BUSINESS-IMPACT) 
- [ANALYSIS](#ANALYSIS) 
- [RESULTS AND VISUALS](#RESULTS-AND-VISUALS) 
- [INSIGHT](#ROOT-CAUSE-INSIGHTS) 
- [RECOMMENDATION](#RECOMMENDATION) 
- [WHY THE FIX WILL WORK](#WHY-THE-FIX-WILL-WORK)

---

## CONTEXT AND PROBLEM

Posh Hairs' business based in Ghana.  
Recently, The Finance and the Operational teams noticed that **the business lacks an accurate profitability model for its most expensive products, leading the business to actively lose profit margin on high-value international orders due to chronic operational failures.

### Problem Statement
 "The company is losing profit margin on high-value international orders. 
This affects the Finance and Operations Teams by Low Profit Margin and unsustainable Logistics cost.
 We need to answer which combinations are profitable and which combinations must be stopped. 
 We need to help them achieve High net profit Margin and identify the combinationscausing the loss, measured by Profit Margin and Logistics cost.
 Solving it will Improve the profit and Logistics decisions become data driven while ignoring it will harm the business finances in the long run"

---

### Stakeholders

| Role | Responsibility |
|------|----------------|
| **Finance Teams** | Oversees financial health and strategy of the business |
| **Operational Teams** | coordinate day-to-day execution the business activities  |
| **Data Analyst** | Diagnose causes, quantify risks, communicate insights |

---

## Dataset Metadata

![Dataset](https://github.com/OluwafunmilayoB12/Project-2/tree/main)


The project uses three CSV files but focuses on specific columns to answer the technical questions.

| Table | Description | Key Columns |
|--------|--------------|-------------|
| **customers** | The detailed customers log | `CustomerID`, `Country_name`, `Preferred-currency` |
| **Products** | Products attributes | `SKUs, `Product`, 'Retail-price_GHS' |
| **Transaction_clean** | Transaction details | `Order_date`, `Revenue_logged`, `marketing_channel` |

---

## 🧾 DECISION TABLE — DATA QUALITY & CLEANING STRATEGY

📎 **Visual Reference:** ![Decision Table]

---

### DATA CLEANING – 3-PHASE PROCESS

**Phase 1 – Numeric Corrections**  
Total_Revenue_Logged column is inconsistent having mixed currency value, convert the values to GHS
CAST(LEFT(Total_Revenue_Logged, LEN(Total_Revenue_Logged) - 3) AS DECIMAL(18,2)) as Revenue_logged .

**Phase 2 – Text Standardization**  
CustomerID, country-name  and SKU were inconsistent in casing and fixed to ensure consistent joins

**Phase 3 – Logical Integrity**  
Dropped duplicated transaction_id of same values across all relevant columns

---

## HYPOTHESIS EXPLORATION

Before analysis, this was tested:

 **Revenue gaps are caused by logistics inefficiencies**

Analyzed performance across country and channel
Checked distribution consistency

→ **False** – no signicant evidence of logistics contraints observed

 **Revenue gaps are diven by demand and pricing factors, not logistics


Applied the **BAIIR** Framework (*Baseline, Analysis, Insight, Impact, Recommendation*) to document the analysis for clarity.

---

## BUSINESS IMPACT

| Metric | Value | Interpretation |
|---------|--------|----------------|
| Revenue | GHS 165.17M| |
| Expected Revenue | GHS 127.30M| |
| Revenue Gap | GHS 37.88M | |
| Revenue Gain | GHS 38.15M | |
| Revenue Loss | GHS 270.18K| |
---

## ANALYSIS

### Queries Used
```sql
-- Baseline trend

WITH Revenue AS (
	SELECT p.Retail_price_GHS, 
			t.Quantity_purchased, 
			t.Revenue_logged,
			e.Rate_to_GHS,
			p.Retail_price_GHS * t.Quantity_purchased as Expected_Revenue_GHS,
			t.revenue_logged * e.rate_to_GHS as converted_revenue_GHS
	FROM Transaction_clean t
	JOIN Products p
	ON t.SKUs = p.SKUs
	INNER JOIN Exchange_rates e
	ON t.currency_code = e.currency_code
	AND YEAR(t.order_Date) = e.year
	)
	SELECT converted_revenue_GHS, 
			Expected_Revenue_GHS,
	(converted_revenue_GHS - Expected_Revenue_GHS)  as Difference
	FROM Revenue;
```

---

## RESULTS AND VISUALS

### VISUAL 1 - Revenue Overview
![Visual 1]
Revenue shows an overall upward trend but drops significantly in Nov 2024 and Nov 2025
### VISUAL 2 Gap Analysis
![Visual 2]
Revenue gap is driven by high impact underperforming segments. Optimize Bottom right combinations and scale top right performers.

### VISUAL 3 - Action Plan and Recommendation
![Visual 3]

---
## IMPACTS
-Actual revenue of GHC 165.17M exceeds the expected benchmark of GHC 127.30M (+29.75% above expected performance), indicating strong overall performance 
-The positive variance of GHC 37.88M +29.75% against expected revenue shows stronger than anticipated demand and execution outcomes
-Revenue gain of GHC 38.15M (+29.97% of expected revenue) shows successful segments driving performance above the forecast baseline 
-Revenue loss of GHS 270.18k (0.21% of expected revenue) represents leakage that reduces efficiency despite overall performance above benchmark

---

### INSIGHTS

- Revenue shows overall growth but with recurring drops in key periods
- Revenue gaps are not linked to logistics constraints
- Underperformance is driven by specific channels and product mix
- Pricing inconsistencies and FX handling contribute to artificial revenue gaps

---
## RECOMMENDATIONS
| Focus Area | Action | Owner | Priority | Expected impact |
|-------------|---------|--------|-----------|-----------|
| Pricing & Revenue | Standardize single base pricing across markets and apply consistent FX conversion with margin buffer | Finance Team | 🔴 High | Recover ~GHC 27K-40.5K from inconsistencies 
| Channel Performance | Reallocate budget to high performing channels and optimize underperforming ones | Marketing | 🔴 High | Increase revenue by ~GHC 10K-25K
| Financial Controls | Automate refund tracking and improve revenue reconciliation | Finance + Operations | 🔴 High | Reduce revenue loss by 10-15% | 
| Product Strategy | Promote top performing products and optimize product mix | Product + Operations | 🟢 Medium |improve revenue by ~GHC 8K-15K
| Strategic Direction | Focus on demand generation rather than operational adjustment | Management | 🟢 Low| improve overall revenue stability


Addressing these issues will improve revenue quality and profit margins, not just top line growth
potentially recover 10-15% of revenue loss (~GHC 27K-40.5K), with additional gains from pricing and channel optimization


## Key Assumptions & Challenges 
 -**FX Rates**: Created 2024-2025 averages

 
	Avg 2024 rate- GHS base currency
		      USD 14.4800 
		      NGN 0.0084  
	Avg 2024 rate- GHS base currency
		      USD 10.5000 
		      NGN 0.0101  
	from public source no original rates provided.

 -**Cost Data**: Net profit cannot be generated directly because cost value is unavailable
 -Incorrect refund application by the operational teams.
