#  Walmart Store Sales Analysis  
**A complete end-to-end SQL + Power BI analytics project**

This project analyzes **Walmart store-level weekly sales**, explores the impact of holidays, temperature, fuel prices, economic indicators, and identifies top-performing stores using SQL, Power BI, and DAX.

The goal of the project is to demonstrate strong **data cleaning**, **SQL analytics**, **visualization**, and **business insight** skills.

---

---

##  Dataset Information

This dataset contains **Walmart weekly sales data** across multiple stores, with variables such as:

- **Store**
- **Date**
- **Weekly_Sales**
- **Holiday_Flag**
- **Temperature**
- **Fuel_Price**
- **CPI**
- **Unemployment**

Source:  
🔗 *Kaggle — Walmart Store Sales*  
(Direct dataset files are excluded due to licensing.)

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **SQL (MySQL)** | Data cleaning, exploration, transformations |
| **Power BI** | Interactive dashboard creation |
| **DAX** | Custom KPIs, aggregation logic |
| **Excel** | (Optional) Data checks |
| **GitHub** | Version control & project documentation |

---

##  1. Data Cleaning (SQL)

Performed rigorous data quality checks including:

### ✔ Completeness  
Checked for missing values across all columns.

### ✔ Uniqueness  
Detected and reviewed duplicate records.

### ✔ Validity  
Verified that numerical values (sales, fuel price, CPI, unemployment) fall within realistic ranges.

### ✔ Consistency  
Standardized date formats and corrected invalid formats (`'-'` → `'/'`).

### ✔ Outlier Detection  
Flagged records where **Weekly_Sales** > average + 3× standard deviation.

### ✔ Type Casting  
Converted `Date` from text → proper `DATE` type.

 All SQL scripts are inside:  
 `sql/walmart_analysis.sql`

---

## 2. Key Analysis Questions Answered

###  **Sales Insights**
- Which year had the highest sales?  
- Which stores performed the best?
- Do holiday weeks produce higher sales?
- How do fuel prices, CPI, and unemployment relate to sales?

###  **Store Performance**
- Top 5 stores by revenue  
- Store rankings using window functions  
- Cumulative revenue contribution  

###  **Economic & Seasonal Trends**
- Does weather (temperature) affect sales?
- How do economic indicators (CPI, unemployment) correlate with sales?
- Seasonal/quarterly patterns identified using CTE + LAG()

---

##  3. Dashboard (Power BI)

The interactive dashboard includes:

### 🔹 **KPIs**
- Total Sales
- Average Weekly Sales
- Best Year
- Total Stores
- Holiday vs Non-Holiday Sales

### 🔹 **Visuals**
- Sales trend over time
- Store-level sales bar chart (Top 5)
- Temperature vs Sales scatter
- Economic indicators (CPI & Unemployment)
- Holiday impact analysis
- Quarterly trend with DAX

### 🔹 Preview  
<img src="dashboard/dashboard_preview.png" width="700"/>

Download the full dashboard here:  
 `dashboard/walmart_dashboard.pbix`

---

##  Key Insights

###  Store Performance
- **Store 20 and Store 4** consistently lead in total sales.
- Some stores are categorized as **Top**, **Average**, and **Low performers** based on total sales.

###  Seasonality
- Sales peak strongly in **November–December**, especially during holiday weeks.

###  Holiday Impact
- Holiday weeks show **higher average weekly sales** compared to non-holiday weeks.

### Temperature Influence
- Sales are higher when temperatures are mild (**46–76°F**).
- Extremely hot or cold days show lower activity.

### Economic Indicators
- Higher unemployment is associated with lower sales.
- Fuel price shows weak negative correlation.

---

##  4. SQL Highlights

This project showcases strong SQL skills:

- Window functions  
- CTEs  
- Ranking  
- Outlier detection  
- Date cleaning  
- Aggregation  
- Case statements  
- Correlation analysis  

Example: Quarterly analysis using LAG():

```sql
WITH quarterly_sales AS (
  SELECT 
      YEAR(Date) AS Year,
      QUARTER(Date) AS Quarter,
      SUM(Weekly_Sales) AS Total_Sales
  FROM walmart_Staging
  GROUP BY Year, Quarter
)
SELECT *,
       LAG(Total_Sales) OVER(ORDER BY Year, Quarter) AS Prev_Quarter_Sales,
       ROUND(((Total_Sales - LAG(Total_Sales) 
       OVER(ORDER BY Year, Quarter)) 
       / LAG(Total_Sales) OVER(ORDER BY Year, Quarter)) * 100, 2) 
       AS Growth_Percentage
FROM quarterly_sales;
