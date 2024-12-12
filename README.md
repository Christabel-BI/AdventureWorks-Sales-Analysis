# AdventureWorks Sales Analysis

## Project Overview

The AdventureWorks Sales Analysis project serves as a testament to the power of data-driven decision-making. This project leverages the AdventureWorks dataset to perform a deep dive into sales performance, cost analysis, and customer preferences. By utilizing industry-standard tools and techniques, the project provides actionable insights that can drive business strategies. From SQL-based data cleaning to Excel-powered visualization, every step of this project reflects meticulous planning and execution. The resulting dashboard and analysis empower stakeholders with a clear understanding of key performance indicators, cost structures, and optimization opportunities.he project demonstrates skills in data extraction, transformation, analysis, and visualization using tools like SQL Server, Power Query, Power Pivot, and Excel.

### Business Question
Write SQL queries and create a dashboard in Excel that contains charts showing:
- What is the Average Total Sales for each product name?
- What is the Total tax amount for each product color?
- What is the Total freight for each product name?
- What is The sum of proportion of the sum of total product cost for each product name ?

### Project Workflow

1. Data Extraction and Loading

- Database: The AdventureWorks dataset was initially loaded into Microsoft SQL Server.

- Data Exploration: SQL was used to clean and explore the dataset, ensuring data integrity and preparing it for analysis.

2. Data Transformation

- Power Query: Cleaned and transformed the data using Power Query in Excel.

- Data Modeling: Established relationships between tables using foreign keys in Power Pivot to enable a robust data model.

3. Calculations and Aggregations

- DAX Calculations: Created calculated columns and measures using DAX (Data Analysis Expressions) to derive meaningful insights:

- Average Total Sales

- Total Tax Amount

- Total Freight Costs

- Pivot Tables: Used pivot tables to aggregate data and facilitate detailed analysis.

4. Data Analysis and Insights

Performed detailed analysis on:

- Product Performance: Identified top-performing products by sales.

- Cost Analysis: Examined freight and tax costs for high-value products.

- Product Diversity: Analyzed product color distribution and proportional costs.

5. Dashboard Creation

- Excel Dashboard: Designed a dynamic and interactive dashboard to visualize key metrics and trends derived from the pivot tables.
https://github.com/Christabel-BI/AdventureWorks-Sales-Analysis/blob/main/Analysis%20Dashboard.png

### File Structure
- <a href= "https://github.com/Christabel-BI/AdventureWorks-Sales-Analysis/blob/main/AdventureWorks%20.xlsx">Downlaod here<a/> 
AdventureWorks.xlsx: Contains the transformed and analyzed data, pivot tables, and the dashboard.

ProductName_TotalSales: Total sales by product name.

ProductColor_TotalTaxAmt: Total tax amounts by product color.

ProductName_TotalFreight: Freight costs by product name.

ProductName_Proportional_cost: Proportional cost analysis for each product.

Pivot Table: Aggregated summaries for analysis.

Dashboard: Visual representation of key insights.

### Key Insights

- High-value Products: Premium cycling gear like the "Road-150" series generated significant sales, highlighting a profitable product line.

- Cost Drivers: Freight and tax costs were particularly high for top-selling products, impacting overall profitability.

- Customer Preferences: Products with specific colors (e.g., Red, Yellow and Black) were more popular, providing segmentation opportunities.

- Product Distribution: A small percentage of products contribute to the majority of sales, indicating a potential 80/20 rule application.

- Profitability Variance: While high sales products bring revenue, some have slimmer margins due to high associated costs.

### Recommendations

- Optimize Cost Management: Analyze and negotiate better freight and tax arrangements for high-revenue products.

- Focus on High-demand Products: Prioritize marketing and stock management for top-selling items like the "Road-150" series.

- Targeted Marketing Campaigns: Leverage customer preferences for specific colors to design targeted promotions and offerings.

- Inventory Management: Implement strategies to maintain optimal inventory levels for high-performing product lines.

- Margin Analysis: Conduct detailed profitability analyses to identify and improve margins for top-selling products.

### Tools and Technologies

- SQL Server: Data exploration and cleaning.

- Power Query: Data extraction and transformation.

- Power Pivot: Data modeling and relationship management.

- DAX: Advanced calculations for insightful metrics.

- Excel: Pivot tables, data visualization, and dashboard creation.

### SQL queries used for Exploratory Data Analysis
SQL
```--Total Sales for each product name

select * from AdventureWork_Sales
select * from AdventureWorks_Products

CREATE VIEW ProductName_TotalSales AS 
select dbo.AdventureWorks_Products.ProductKey, dbo.AdventureWorks_Products.ProductName,sum(dbo.AdventureWork_Sales.SalesAmount)
       as TotalSales from dbo.AdventureWorks_Products
inner join dbo.AdventureWork_Sales on dbo.AdventureWorks_Products.ProductKey = dbo.AdventureWork_Sales.ProductKey
GROUP BY dbo.AdventureWorks_Products.ProductName,dbo.AdventureWorks_Products.ProductKey

CREATE VIEW ProductColor_TotalTaxAmt AS 
select dbo.AdventureWorks_Products.ProductKey,dbo.AdventureWorks_Products.ProductColor,sum(dbo.AdventureWork_Sales.TaxAmt)
       as TotalTaxAmt from dbo.AdventureWorks_Products
inner join dbo.AdventureWork_Sales on dbo.AdventureWorks_Products.ProductKey = dbo.AdventureWork_Sales.ProductKey
GROUP BY dbo.AdventureWorks_Products.ProductColor,dbo.AdventureWorks_Products.ProductKey

CREATE VIEW ProductName_TotalFreight AS 
select dbo.AdventureWorks_Products.ProductKey,dbo.AdventureWorks_Products.ProductName,sum(dbo.AdventureWork_Sales.Freight)
       as TotalFreight from dbo.AdventureWorks_Products
inner join dbo.AdventureWork_Sales on dbo.AdventureWorks_Products.ProductKey = dbo.AdventureWork_Sales.ProductKey
GROUP BY dbo.AdventureWorks_Products.ProductName,dbo.AdventureWorks_Products.ProductKey


CREATE VIEW ProductName_Proportional_cost AS 
WITH ProductCostCTE AS (
    -- Step 1: Get total cost for each product
    SELECT 
        ProductName,ProductKey,
        SUM(ProductCost) AS TotalProductCost
    FROM 
        dbo.AdventureWorks_Products
    GROUP BY 
        ProductName,ProductKey
),
TotalCostCTE AS (
    -- Step 2: Calculate the overall total product cost
    SELECT 
        SUM(TotalProductCost) AS OverallTotalCost
    FROM 
        ProductCostCTE
)
-- Step 3 & 4: Calculate the proportion of each product and sum the proportions
SELECT ProductName,ProductKey,
    SUM(TotalProductCost / OverallTotalCost) AS SumOfProportions
FROM 
    ProductCostCTE, TotalCostCTE 
	group by ProductName,ProductKey;

	select * from ProductName_Proportional_cost

-- Question 3

select * from dbo.AdventureWorks_Territories
select * from dbo.AdventureWork_Sales

CREATE VIEW Territories_Sales AS
select AdventureWorks_Territories.SalesTerritoryKey , AdventureWorks_Territories.Country, 
		AdventureWorks_Territories.Region , sum(dbo.AdventureWork_Sales.SalesAmount) as TotalSalesAmount, 
		sum(dbo.AdventureWork_Sales.TaxAmt) as TotalTaxAmt, sum(dbo.AdventureWork_Sales.Freight) as TotalFreight 
	From dbo.AdventureWorks_Territories 
join dbo.AdventureWork_Sales on dbo.AdventureWorks_Territories.SalesTerritoryKey = dbo.AdventureWork_Sales.SalesTerritoryKey
group by AdventureWorks_Territories.SalesTerritoryKey,AdventureWorks_Territories.Country,AdventureWorks_Territories.Region```


