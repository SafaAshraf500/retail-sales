# Retail Sales Analytics Dashboard

An interactive five-page **Power BI** report that turns eight raw sales tables into clear answers about sales, products, customers, and countries. Built as part of a Data Engineering Bootcamp project.

## Overview

The report is for a retail store that sells electronics, home appliances, clothing, and books in four countries. It covers the full BI workflow: data profiling and cleaning in Power Query, a star-schema data model, DAX measures, and a report with one consistent design.

| Metric | Value |
|---|---|
| Total sales | 90.58M |
| Total profit | 26.63M (29% margin) |
| Orders | about 49.5K |
| Customers | 2,000 |
| Products | 80, in 4 categories |
| Markets | 4 countries, 12 cities |

## Report pages

| Page | Question it answers |
|---|---|
| **Home Page** | Where do I start? Navigation to all analysis pages |
| **Executive Overview** | How are sales and profit performing? |
| **Product Analysis** | What makes money and what loses it? |
| **Customer Analysis** | Who buys, and who comes back? |
| **Country & Location** | Where are we selling? |

## Tools

- Microsoft Excel (source data)
- Power Query (M) for data preparation
- DAX for measures
- Power BI Desktop for the data model and report

## Data preparation

The data was profiled and cleaned in Power Query. Main steps:

- Removed duplicate invoice rows.
- Filtered out rows with no `CustomerID` and `ProductID`.
- Replaced the short original calendar with a new date table built from the first to the last year of sales.
- Created `Product Label` and `Customer Label` (name + ID), because names repeat.
- Grouped missing ages as "Unknown" instead of dropping the customers.
- Merged category and location tables into the dimensions to remove the snowflake.

## Data model

A **star schema**: `Fact_Sales` in the center, connected to `Dim_Customer`, `Dim_Product`, and `Dim_Date`.

- All relationships are one-to-many, and filters flow from the dimension to the fact table.
- `Dim_Customer[RegistrationDate]` has an inactive relationship to `Dim_Date`, used only by the *New Customers* measure through `USERELATIONSHIP`.

## Key DAX measures

All measures are stored in a `_Measures` table.

```dax
Total Sales   = SUM ( Fact_Sales[TotalAmount] )
Total Cost    = SUMX ( Fact_Sales, Fact_Sales[Quantity] * RELATED ( Dim_Product[Cost] ) )
Total Profit  = [Total Sales] - [Total Cost]
Profit Margin = DIVIDE ( [Total Profit], [Total Sales] )
Sales LY      = CALCULATE ( [Total Sales], SAMEPERIODLASTYEAR ( Dim_Date[Date] ) )
YoY %         = DIVIDE ( [Total Sales] - [Sales LY], [Sales LY] )
```

Other measures include Active Customers, New Customers, Repeat Customers, Avg Orders, Top Product, Top Country, and Lowest Margin.

## Key insights

- Electronics (52%) and Home Appliances (45%) generate almost all revenue.
- Sales are balanced across the four countries, so the business does not depend on one market.
- A few premium products, such as MacBook Pro and Dell Inspiron, lead sales.

## Repository contents

| File | Description |
|---|---|
| `Task_19.pbix` | The Power BI report |
| `PowerBI_Project_Documentation.pdf` | Full project documentation |

## How to open

1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
2. Download the `.pbix` file from this repository.
3. Open it in Power BI Desktop. If asked, update the data source path to your local copy of the Excel files.

## Notes

- The data is synthetic, so the conclusions are for practice only.
- "Country" means the country where the customer lives, because the data has no store location.
- The sales trend drops in the last months, which probably means those months are incomplete.

## Author

**Safa**, [@SafaAshraf500](https://github.com/SafaAshraf500)
