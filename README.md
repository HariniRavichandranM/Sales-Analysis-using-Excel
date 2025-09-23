#  Sales-Analysis-using-Excel
Analyses coffee bean sales with Excel, Power Query, and Power Pivot to deliver interactive dashboards showing sales trends, product popularity, customer behavior and regional insights.
Coffee Beans Sales Dashboard

Date: 03/03/2025
Data Source: Kaggle (Coffee Sales Dataset)
Tools: Excel, Power Query, Power Pivot, DAX

# Project Objective

Analyse coffee bean sales and customer behaviour to build an interactive dashboard with KPIs, trends, and insights for business decision-making.

 # Data Model

Products: Product ID, Coffee Type, Roast, Size, Unit Price, Profit

Orders: Order ID, Order Date, Customer ID, Product ID, Quantity

Customers: Customer ID, Name, Email, Phone, Address, City, Country, Loyalty Card

# Key Business Questions

When do people buy? → busiest days/hours, seasonal trends

How much do they spend? → AOV, holiday spikes, Standard Deviation in Country

Which price ranges sell? → budget vs premium by region

What do people buy? → top coffee types, roast, size, revenue contributors


# Data Cleaning & Preparation
Removed duplicates & empty columns

Filled missing values (Email = "unknown@example.com", Phone = "0000000000")

Fixed relationship errors (removed blank keys)

Added Sales column via VLOOKUP

All transformations are done in **Power Query** because it ensures data consistency, preserves changes on refresh, and avoids manual errors in the Excel sheet. 
Power Query does dynamic, repeatable cleaning and shaping of raw data before it enters the data model.

# Modeling & Measures

Cleaned data feeds into a **star schema** data model with fact and dimension tables (Orders as fact, Products and Customers as dimensions ).  
A dedicated **Date Table** was created in Power Query to support time-based analysis and enable advanced time intelligence in measures.
   - Enables creation of **Power Pivot measures** for key metrics: Total Sales, Total Profit, Profit %, AOV, Growth %, Gross Profit %, Weekend Sales, Median Order Value, Repeat Customers.  
   - Allows proper **relationships between tables** for accurate aggregations and time intelligence calculations.  

# Key Measures:
## 🛠 Modeling & Measures

### Key DAX Measures

- **Total Sales:**  
```DAX
Total Sales = SUM(Orders[Total Sales])
Total Profit:

Total Gross Profit = SUM(Orders[Profit])

Profit % = DIVIDE(
    SUMX(Orders, Orders[Quantity] * RELATED(Products[Profit])),
    SUMX(Orders, Orders[Quantity] * RELATED(Products[Unit Price]))
)

AOV = DIVIDE([Total Sales], DISTINCTCOUNT(Orders[Order ID]))

Gross Profit % = DIVIDE([Total Profit], [Total Sales])

Number of Orders = COUNTROWS(RELATEDTABLE(Orders))

Sales Growth % := 
DIVIDE(
    [Total Sales] - CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date 1'[Date])),
    CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date 1'[Date]))
) * 100
Weekend Sales := 
CALCULATE(
    [Total Sales],
    FILTER('Date 1', 'Date 1'[Weekday Name] IN {"Saturday", "Sunday"})
)

Weekend Orders := 
CALCULATE(
    COUNTROWS(RELATEDTABLE(Orders)),
    FILTER('Date 1', 'Date 1'[Weekday Name] IN {"Saturday", "Sunday"})
)

Median Order Value := MEDIANX(VALUES(Orders[Order ID]), [Total Sales])

Repeat Customer (Calculated Column):
Repeat Customer = 
IF(
    CALCULATE(COUNTROWS(Orders), FILTER(Orders, Orders[Customer ID] = Customers[Customer ID])) > 1,
    TRUE,
    FALSE
)
```

# Statistical Analysis

Average sales by region → Identify top/bottom markets

Standard deviation → Measure sales volatility

Seasonality → Monthly sales trends

Customer spend patterns → Loyalty vs non-loyalty comparison

 # Analysis & Visuals

- **KPIs:** Sales, Profit, Growth %, AOV  
- **Trends:** Sales by year, month, weekday, hour  
- **Products:** Coffee type, roast, size, top N products  
- **Customers:** Loyalty vs non-loyalty, repeat vs new buyers  
- **Regions:** Sales distribution, standard deviation, high/low performers  
- **Filters/Slicers:** Country, city, year, quarter, category  
- **Interactive Elements:** Icons for **Dashboard**, **Tables**, **Insights**, and **Contact**

##  Final Dashboard

- **Executive View & KPIs:**  
  - Total Sales  
  - Total Profit  
  - Gross Profit %  
  - Sales Growth %  
  - Average Order Value (AOV)  
  - Median Order Value  
  - Weekend Sales & Orders  

- **Interactive Elements:** Charts and slicers for time, region, product, and loyalty  

- **Key Insights Visualised                                                                                                                                    :**  
  - Sales growth over time  
  - Seasonal trends  
  - Sales and profit by country  
  - Sales and profit by product  
  - Pivot Tables - AOV and Sales SD by country and Loyalty vs Non-Loyalty



