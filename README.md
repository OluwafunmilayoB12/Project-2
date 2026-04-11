#  Posh Hairs Revenue Performance Analysis
*A Data-Driven Investigation into Revenue performance, gap analysis and contribution to the business finance*

---
## TABLE OF CONTENT
- [CONTEXT AND PROBLEM](#CONTEXT-AND-PROBLEM) 
- [DATASET METADATA](#DATASET-METADATA) 
- [DATA QUALITY AND CLEANING STRATEGY](#DATA-QUALITY-AND-CLEANING-STRATEGY) 
- [HYPOTHESIS EXPLORATION](#HYPOTHESIS-EXPLORATION) 
- [BASELINE](#BASELINE) 
- [ANALYSIS](#ANALYSIS) 
- [RESULTS AND VISUALS](#RESULTS-AND-VISUALS)
- [IMPACTS](#IMPACTS) 
- [INSIGHT](#INSIGHTS) 
- [RECOMMENDATIONS](#RECOMMENDATIONS) 
- [Key Assumptions & Challenges](#Key-Assumptions-And-Challenges )

---

## CONTEXT AND PROBLEM

Posh Hairs' business based in Ghana.  
Recently, The Finance and the Operational teams noticed that the business lacks an accurate profitability model for its most expensive products, leading the business to actively lose profit margin on high-value international orders due to chronic operational failures.

### Problem Statement
 "The company is losing profit margin on high-value international orders. 
This affects the Finance and Operations Teams by Low Profit Margin and unsustainable Logistics cost.
 We need to answer which combinations are profitable and which combinations must be stopped. 
 We need to help identify the combinations causing the loss, measured by revenue gap and performance drivers.
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
| **Products** | Products attributes | `SKUs',`Product`, 'Retail_price_GHS' |
| **Transaction_clean** | Transaction details | `Order_date`, `Revenue_logged`, `marketing_channel` |

---
Tools & Skills Used

• SQL (Data Cleaning, quality & Analysis)

• Power BI (Dashboard & Visualization)

• Data Modeling & DAX


## 🧾 DECISION TABLE — DATA QUALITY & CLEANING STRATEGY

📎 **Visual Reference:** ![Decision Table](https://github.com/OluwafunmilayoB12/Project-2/blob/main/Posh%20-%20data%20quality%201.png)
                        ![Decision Table](https://github.com/OluwafunmilayoB12/Project-2/blob/main/Posh%20-%20data%20quality%202.png)

---

### DATA CLEANING – 3-PHASE PROCESS

**Phase 1 – Numeric Corrections**  
Total_Revenue_Logged column is inconsistent having mixed currency value, convert the values to GHS
CAST(LEFT(Total_Revenue_Logged, LEN(Total_Revenue_Logged) - 3) AS DECIMAL(18,2)) as Revenue_logged .

**Phase 2 – Text Standardization**  
CustomerID, country-name  and SKU were inconsistent in casing and fixed to ensure consistent joins

**Phase 3 – Logical Integrity**  
Dropped duplicated transaction_id of same values across all relevant columns

Before data quality

![Messy data](https://github.com/OluwafunmilayoB12/Project-2/blob/main/messy%20data.png)

After data quality

![Clean data](https://github.com/OluwafunmilayoB12/Project-2/blob/main/clean%20data.png)

---

## HYPOTHESIS EXPLORATION

Before analysis, this was tested:

 **Revenue gaps are caused by logistics inefficiencies**

Analyzed performance across country and channel
Checked distribution consistency

→ **False** – no signicant evidence of logistics constraints observed

 **Revenue gaps are diven by demand and pricing factors, not logistics


Applied the **BAIIR** Framework (*Baseline, Analysis, Insight, Impact, Recommendation*) to document the analysis for clarity.

---

## BASELINE

| Metric | Value | Interpretation |
|---------|--------|----------------|
| Revenue | GHS 165.17M|Indicates strong business growth and performance above expectations |
| Expected Revenue | GHS 127.30M| Serves as benchmark for evaluating actual performance |
| Revenue Gap | GHS 37.88M | Positive gap confirms overall performance exceeds targets |
| Revenue Gain | GHS 38.15M | Driven by strong-performing periods and channels|
| Revenue Loss | GHS 270.18K| Limited to specific periods (Nov 2024 and Nov 2025), suggesting temporary gaps likely driven by channel or product underperformance rather than overall decline.|
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
![Visual 1](https://github.com/OluwafunmilayoB12/Project-2/blob/main/visual%20posh%20i.png)

Revenue shows an overall upward trend but drops significantly in Nov 2024 and Nov 2025

### VISUAL 2 Gap Analysis
![Visual 2](https://github.com/OluwafunmilayoB12/Project-2/blob/main/visual%20posh%20ii.png)

Revenue gap is driven by high impact underperforming segments. **Optimize** Bottom right combinations and **scale** top right performers**.
- Top Right (High Revenue + positive gap) → Scale
- Top Left  (Low Revenue + positive gap) → Need improvement
- Bottom Right (High Revenue + Negative gap) → Fix problem & optimize
  
 (High Revenue + Negative gap) this combo generate lot of revenue gap but are underperforming expectaction → it represent the biggest financial risk

### VISUAL 3 - Action Plan and Recommendation
![Visual 3](https://github.com/OluwafunmilayoB12/Project-2/blob/main/visual%20posh%20iii.png)

---

## INSIGHTS

- Revenue shows overall growth but with recurring drops in Nov 2024 and Nov 2025.
- Revenue gaps are not linked to logistics constraints but are influenced by: demand strength, marketing channel effectiveness and operational inconsistencies. 
- Underperformance is driven by specific channels and product mix. 
- Pricing inconsistencies and FX handling contribute to artificial revenue gaps. 


---

## IMPACTS
- Actual revenue of GHC 165.17M exceeds the expected benchmark of GHC 127.30M (+29.75% above expected performance), indicating strong overall performance.
- The positive variance of GHC 37.88M +29.75% against expected revenue shows stronger than anticipated demand and execution outcomes.
- Revenue gain of GHC 38.15M (+29.97% of expected revenue) shows successful segments driving performance above the forecast baseline.
- Revenue loss of GHS 270.18k (0.21% of expected revenue) represents leakage that reduces efficiency despite overall performance above benchmark.

---

## RECOMMENDATIONS
| Focus Area | Action | Owner | Priority | Expected impact |
|-------------|---------|--------|-----------|-----------|
| Pricing & Revenue | Standardize single base pricing across markets and apply consistent FX conversion with margin buffer | Finance Team | 🔴 High | Recover ~GHC 27K-40.5K from inconsistencies 
| Channel Performance | Reallocate budget to high performing channels and optimize underperforming ones | Marketing | 🔴 High | Increase revenue by ~GHC 10K-25K
| Financial Controls | Automate refund tracking and improve revenue reconciliation | Finance + Operations | 🔴 High | Reduce revenue loss by 10-15% | 
| Product Strategy | Promote top performing products and optimize product mix | Product + Operations | 🟢 Medium |improve revenue by ~GHC 8K-15K
| Strategic Direction | Focus on demand generation rather than operational adjustment | Management | 🟢 Low| improve overall revenue stability


Addressing these issues will improve revenue quality and profit margins, not just top line growth.
Potentially recover 10-15% of revenue loss (~GHC 27K-40.5K), with additional gains from pricing and channel optimization


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
