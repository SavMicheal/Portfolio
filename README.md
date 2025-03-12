# Retail_Store_Inventory

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data cleaning](#data-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)

### Project Overview

This data analysis project aims to provide insights into the sales performance of a retail store over a period of time. By analyzing various aspect of the sales data, we seek to identify trends, make data driven recommendation and gain deeper understanding of thr company's performance

### Data Sources
The primary dataset used for this analysis is the Retail_store_inventory.csv,  containing detailed information about each sales by the company

### Tools 

- Sql server

### Data cleaning

In the initial data prepararion phase, we performed the following tasks:
1. Data loading and inspection
2. Data cleaning and formatting

### Exploratory Data Analysis

EDA Involved exploring the sales data to answer key question, such as:

- which products are top sellers
- Total discounts issued to customers per category
- Comparing Company Sales And Competitor Sales With Same UnitSold
- What weather encourages sales

### Data Analysis

Include some interesting code/features worked with

--Top Sales By Category
```
Select Top 1 Convert(Date, Date) As Date, Category, Units_Sold * Price - Discount As Sales From RetailStoreInventory
Order By Sales Desc;
```

--Compare Company Sales And Competitor Sales With Same UnitSold
```
With SalesDifference (Date, Category, Discount, CompanySales, CompetitorSales)
As
(Select Convert(Date, Date) As Date, Category, Discount, Units_Sold * Price, Units_Sold * Competitor_Pricing 
From RetailStoreInventory)
Select Date, Category, CompanySales, CompetitorSales,
Case
When CompanySales > CompetitorSales Then Round((CompanySales - Discount) - CompetitorSales, 1)
When CompanySales < CompetitorSales Then Round((CompanySales - Discount) - CompetitorSales, 1)
Else 0
End As SalesDiff
From SalesDifference
Order By CompanySales Desc, CompetitorSales Desc
```

--What weather Condition Encourage Sales
```
select Seasonality, Sum(Units_Sold * Price) As UnitSales From RetailStoreInventory
Group By 
	Grouping Sets(
		(Seasonality),
		 ()
		 )
```

--Sales By Category, Region, Weather_Condition, Seasonality
```
Select Category, Region, Weather_Condition, Seasonality, Sum(Units_Sold * Price) From RetailStoreInventory
Group By RollUp (Category, Region, Weather_Condition, Seasonality)
select * from RetailStoreInventory
```

--Total Discount issued To Customers Per Category
```
Select Category, Region, Weather_Condition, Seasonality, Sum(Cast(Discount As Int)) As TotalDiscount From RetailStoreInventory
Group By RollUp (Category, Region, Weather_Condition, Seasonality)
```

### Results
1. The company highest sales by category is Furniture
2. the total discount amount issued to the customers worth  $731695
3. winter encourages sales

### Recommendations
- More investment should be allocated in the furniture category to maximize revenue
- Lesser discount should be given to customer
