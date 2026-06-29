# Bike Sales Data Warehouse

**Term Project | BAN545 — Data Warehousing | Master of Science in Business Analytics | Northern Arizona University**

---

## Project Overview

This project involves designing and building a dimensional data warehouse for bike sales data using Microsoft Access. The warehouse follows a classic star schema with one fact table and three dimension tables, enabling multi-dimensional analysis of sales across customers, products, dates, and geographies.

Analytical queries were built on top of the warehouse to surface insights around sales performance by category, country, customer demographics, and time periods.

---

## Dataset Summary

| Field | Details |
|---|---|
| Source Records | 499 sales transactions |
| Time Period | 2011 |
| Countries | Australia, Canada, France, Germany, United Kingdom, United States |
| Product Categories | Bikes (Mountain Bikes, Road Bikes) |
| Customer Fields | Age, Gender, Country, State |
| Sales Fields | Order Quantity, Unit Price, Total Sales |

---

## Data Warehouse Schema (Star Schema)

```
                    ┌─────────────────┐
                    │  DataDimension  │
                    │  DateID (PK)    │
                    │  Date, Month,   │
                    │  Year, Quarter, │
                    │  Weekday/Weekend│
                    └────────┬────────┘
                             │
┌──────────────────┐   ┌─────▼──────────┐   ┌───────────────────┐
│ ProductDimension │   │   SalesFact    │   │ CustomerDimension  │
│ ProductID (PK)   ├───│ SalesID (PK)   ├───│ CustomerID (PK)    │
│ Product_Category │   │ DateID (FK)    │   │ Customer_Age       │
│ Sub_Category     │   │ ProductID (FK) │   │ Customer_Gender    │
│ Product          │   │ CustomerID(FK) │   │ Customer_Country   │
└──────────────────┘   │ Order_Quantity │   │ Customer_State     │
                       │ Unit_Price     │   └───────────────────┘
                       │ Total_Sales    │
                       └────────────────┘
```

**Dimension Tables:**
- **DataDimension** — 15 date attributes including Month, Quarter, Week Number, Weekday/Weekend, Day of Week
- **ProductDimension** — Product category, sub-category, and product name
- **CustomerDimension** — Customer age, gender, country, and state

**Fact Table:**
- **SalesFact** — Transactional grain; links all three dimensions with Order Quantity, Unit Price, and Total Sales

---

## Key Technical Challenges Solved

**Serial Date Conversion** — The raw dataset stored dates as Excel serial numbers (e.g., 40544 for Jan 1, 2011). Resolved using Excel's `TEXT` function to convert and map correctly to `DateKey` in SalesFact.

**Dimension Deduplication** — Initial import of the DataDimension table produced redundant date entries. Resolved using `SELECT DISTINCT` queries in Access to ensure one record per unique date.

**Product Category Derivation** — Inconsistent `Product_Category` data in the source was corrected by deriving categories from the `Sub_Category` field using an Access update query.

**Foreign Key Standardization** — Date key format mismatches between SalesFact and DataDimension were resolved by standardizing formats and running corrective queries to assign proper foreign keys.

**Age Binning** — CustomerDimension was enriched with age group bins to enable demographic analysis (e.g., average order quantity by age group and gender).

---

## Analytical Queries Built

- **Total Sales by Category & Country** — Revenue breakdown across product categories and customer geographies
- **Sales by Sub-Category in the US** — Drill-down into Mountain Bikes vs. Road Bikes for US customers
- **Avg Quantity by Gender & Age** — Customer demographic analysis using age bins joined with SalesFact
- **Sales by Time Period** — Trend analysis using DataDimension attributes (Quarter, Month, Weekday/Weekend)

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| **Microsoft Access** | Data warehouse build, star schema design, SQL queries |
| **Excel** | Source data, dimension table preparation, date transformation |
| **SQL (Access dialect)** | SELECT DISTINCT, UPDATE, JOIN queries across fact and dimension tables |

---

## Repository Contents

| File | Description |
|---|---|
| `Bikes_Data_for_Data_Warehouse.xlsx` | Source dataset + all 4 warehouse tables (DataDimension, ProductDimension, CustomerDimension, SalesFact) |
| `Data_Warehouse.accdb` | Microsoft Access data warehouse file with star schema and analytical queries |
| `BAN545_Lessons_Learned_Report.docx` | Reflection on challenges, solutions, and improvements for this project |

> **To open the .accdb file:** Microsoft Access is required (part of Microsoft Office). If you don't have Access, the full dimensional model and data are also available in the Excel file.

---

## Author

**Suhel Shaik (Larkin)**
M.S. Business Analytics | Northern Arizona University
[Portfolio](https://suhelshaikportfolio.netlify.app)
