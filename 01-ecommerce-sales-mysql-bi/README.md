# E-commerce Sales Analysis with MySQL & BI (Online Retail II)

## 1. Project Overview

This project analyzes transactional e-commerce data from the **Online Retail II** dataset using **MySQL** and basic **BI tools**. The goal is to answer key business questions about revenue trends, top-performing products, and customer purchasing behavior for an online retail company. The focus is on **data analytics**, not backend development.

---

## 2. Problem Statement

An online retailer wants to better understand how its sales, products, and customers perform over time so that it can make data-driven decisions on marketing, inventory, and customer retention. In this project, I use SQL to explore the following business questions:

- What is the monthly and yearly revenue trend?
- Which products and product groups generate the most revenue?
- What is the average order value (AOV) across different segments?
- How do first-time and repeat customers differ in their purchasing behavior?
- Which countries contribute the most to overall sales?

---

## 3. Dataset

**Name:** Online Retail II  
**Source:** UCI Machine Learning Repository (also available as a Kaggle mirror of the same dataset).  
**Description:** This dataset contains a list of all transactions occurring between 2009–2011 for a UK-based online retail company that mainly sells unique all-occasion gifts. [web:27][web:31][web:32]

Typical fields include:
- **Invoice / InvoiceNo:** Unique identifier for each transaction (invoice).
- **StockCode / ProductCode:** Product identifier.
- **Description:** Product description.
- **Quantity:** Number of units purchased.
- **InvoiceDate:** Date and time of the transaction.
- **UnitPrice:** Price per unit.
- **CustomerID:** Unique identifier for each customer.
- **Country:** Country where the customer resides. [web:27][web:31][web:32]

For this project, the raw data is not stored directly in the repository.  
To reproduce the analysis:
1. Download **Online Retail II** from the UCI repository or its Kaggle mirror.
2. Place the raw file(s) into `data/raw/` (e.g. `data/raw/online_retail_II.xlsx`).

---

## 4. Database Schema & Folder Structure

### 4.1 Logical Schema

The original flat file is modeled into a relational schema to make analysis easier and more realistic for SQL and BI reporting. A simplified schema includes tables such as:

- `customers` – One row per customer, including `customer_id`, `country`, and optional derived attributes.
- `products` – One row per product, including `product_id`, `stock_code`, `description`, and `unit_price`.
- `orders` – One row per invoice/order, including `order_id`, `invoice_no`, `order_date`, `customer_id`.
- `order_items` – Line items for each order, including `order_item_id`, `order_id`, `product_id`, `quantity`, `line_amount`.
- (Optional) `calendar` – A date dimension for time-based analysis.

The SQL script `sql/schemas.sql` creates the database and these core tables.

> **Screenshot suggestion:** Insert an ERD or schema diagram image here showing the relationships between `customers`, `products`, `orders`, and `order_items`.

### 4.2 Project Folder Structure

```text
01-ecommerce-sales-mysql-bi/
│
├── sql/
│   ├── schemas.sql          # DDL: database and table creation
│   └── analysis_kpis.sql    # SQL queries for metrics and insights
│
├── data/
│   └── raw/
│       └── .gitkeep         # Place raw dataset files here (not tracked in Git)
│
├── docs/
│   └── notes.md             # Design decisions, assumptions, and technical notes
│
├── bi/
│   ├── screenshots/         # BI / dashboard / query result screenshots
│   │   └── notes.md         # Description of each screenshot
│   └── notes.md             # BI design notes and layout ideas
│
└── README.md                # This project overview and documentation
```

---

## 5. SQL Analysis & KPIs

All analysis queries are stored in `sql/analysis_kpis.sql`. The focus is on **business KPIs** and **exploratory analysis**, not SQL tricks.

Key analysis themes:

- **Revenue & Growth**
  - Total revenue over time (daily, monthly, yearly).
  - Monthly revenue trend to detect seasonality.
  - Revenue by country and by product.

- **Order-Level Metrics**
  - Number of orders by period.
  - Average order value (AOV).
  - Distribution of order sizes (small vs large orders).

- **Product Performance**
  - Top 10 products by revenue.
  - Top 10 products by quantity sold.
  - Revenue by product category or description keywords.

- **Customer Behavior**
  - Number of unique customers over time.
  - New vs repeat customers.
  - Revenue contribution from repeat customers.
  - Average revenue per customer.

Each query is written to be readable, with comments describing the purpose of the KPI it calculates.

> **Screenshot suggestion:** Include 1–3 result table screenshots here (e.g., monthly revenue, top products, new vs repeat customer revenue).

---

## 6. Business Insights

This section summarizes the main findings from the SQL analysis in plain language. The exact numbers will depend on the final queries and filters you apply, but typical insights from this dataset might include:

- Revenue exhibits clear seasonal patterns, with noticeable peaks in certain months (e.g., around the holiday season).
- A relatively small group of products contributes a large share of total revenue, indicating a strong **top-seller** effect.
- Customers from certain countries (e.g., the UK) account for the majority of sales, while other regions contribute smaller but potentially growing segments.
- Repeat customers tend to have a higher average order value compared to first-time buyers, highlighting the importance of customer retention.

> After you finalize your analysis, update this section with your specific findings and, if you like, include numerical summaries (e.g., “Top 10 products contribute X% of total revenue”).

---

## 7. Power BI / BI Dashboard

After exporting aggregated tables or connecting Power BI directly to MySQL, a simple BI dashboard can be built to visualize the KPIs.

Example dashboard elements:

- **Overview page**
  - Cards for Total Revenue, Total Orders, Total Customers, and AOV.
  - Line chart for monthly revenue over time.

- **Product performance page**
  - Bar chart of top products by revenue.
  - Bar/treemap of revenue by country or product group.

- **Customer behavior page**
  - New vs repeat customer revenue.
  - Distribution of orders per customer.

BI-related notes and design decisions are recorded in `bi/notes.md`, and static images of the dashboard are saved in `bi/screenshots/`.

> **Screenshot suggestion:** Add 1–2 dashboard images here (e.g., `overview_dashboard.png`, `top_products_dashboard.png`).

---

## 8. How to Reproduce

Follow these steps to recreate the analysis:

1. **Clone this repository**

   ```bash
   git clone <repo-url>
   cd 01-ecommerce-sales-mysql-bi
   ```

2. **Download the dataset**

   - Download **Online Retail II** from the UCI Machine Learning Repository (or Kaggle mirror).
   - Save the raw file(s) into `data/raw/`.

3. **Create the database and tables**

   - Open your MySQL client (e.g., MySQL Workbench).
   - Run `sql/schemas.sql` to create the database and tables.

4. **Load the data**

   - Import the Online Retail II data into the appropriate tables.
   - This may involve:
     - Cleaning column names.
     - Mapping fields (e.g., Invoice/CustomerID) to your schema.
     - Handling missing `CustomerID` values and cancelled invoices.

5. **Run the analysis queries**

   - Open `sql/analysis_kpis.sql`.
   - Execute the queries to generate revenue, product, and customer metrics.

6. **(Optional) Build the BI dashboard**

   - Connect Power BI (or another BI tool) to your MySQL database or to exported CSVs.
   - Rebuild the visuals described in section 7.

---

## 9. Assumptions & Data Quality Notes

Some common assumptions and data quality considerations for Online Retail II:

- Orders with negative quantities or specific invoice patterns may indicate **returns** or **cancellations** and may be filtered or treated separately.
- Missing `CustomerID` values may represent unregistered or guest customers and might be excluded from certain customer-level analyses.
- Currency is assumed to be **GBP**, as per the dataset’s original description.
- Timezone and date fields are taken as-is from the source without additional timezone conversion.

Additional technical notes, transformations, and assumptions are documented in `docs/notes.md`.

---

## 10. Future Improvements

Potential next steps to extend this project:

- Implement customer segmentation (e.g., RFM analysis) to classify high-value customers.
- Analyze product returns and their impact on net revenue.
- Add cohort analysis to study customer retention over time.
- Build more advanced dashboards with filters by country, timeframe, and product groups.
- Automate data refresh and reporting using scheduled jobs or scripts.

---

### Contact

If you have feedback or suggestions on how to improve this analysis or the data modeling, feel free to open an issue or reach out.
