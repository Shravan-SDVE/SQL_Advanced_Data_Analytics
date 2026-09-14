# 📊 SQL Advanced Data Analytics

Advanced SQL analytics project built on a small AdventureWorks-style **Gold Layer** sales dataset. It goes beyond basic querying to demonstrate the analytical SQL patterns used in real-world BI work: trend analysis, cumulative tracking, performance benchmarking, part-to-whole contribution, segmentation, and consolidated reporting views.

📄 Full write-up with explanations, queries, and result screenshots: **[SQL_Advanced_Analytics_Documentation.docx](./docs/SQL_Advanced_Analytics_Documentation.docx)**

---

## 🧱 Data Model

The project sits on top of three curated Gold Layer tables in a simple star schema:

| Table | Description |
|---|---|
| `gold.fact_sales` | Grain: one row per order line — `order_number`, `product_key`, `customer_key`, `order_date`, `shipping_date`, `due_date`, `sales_amount`, `quantity`, `price` |
| `gold.dim_customers` | Customer master — `customer_key`, `customer_id`, `customer_number`, `first_name`, `last_name`, `country`, `marital_status`, `gender`, `birthdate`, `create_date` |
| `gold.dim_products` | Product master — `product_key`, `product_id`, `product_number`, `product_name`, `category`, `subcategory`, `maintenance`, `cost`, `product_line`, `start_date` |

`fact_sales` joins to both dimensions via `product_key` and `customer_key`.

## 🔍 Analyses Covered

| # | Script | Technique | Business Question | Key SQL Constructs |
|---|---|---|---|---|
| 1 | `01_change_over_time_analysis.sql` | Change-Over-Time | Total sales by year/month, seasonality | `YEAR()`, `MONTH()`, `DATETRUNC()`, `FORMAT()`, `SUM()`, `AVG()` |
| 2 | `02_cumulative_analysis.sql` | Cumulative | Running total sales, moving average price | `SUM() OVER()`, `AVG() OVER()` |
| 3 | `03_performance_analysis.sql` | Performance (YoY) | Current vs. average / vs. previous year sales | `LAG()`, `AVG() OVER()`, `CASE` |
| 4 | `05_part_to_whole_analysis.sql` | Part-to-Whole | % contribution of each category to total sales | `SUM() OVER()`, `CAST()`, `ROUND()` |
| 5 | `04_data_segmentation.sql` | Segmentation | Products by cost range; customers by spend behavior | `CASE WHEN`, `GROUP BY` |
| 6 | `06_report_customers.sql`, `07_report_products.sql` | Reporting | Consolidated customer & product views with KPIs | CTEs, `JOIN`, `CASE`, `DATEDIFF()`, Views |

### 1. Change-Over-Time Analysis
Tracks how a measure evolves over time to identify trends and seasonality (e.g. Total Sales by Year, Average Cost by Month).

### 2. Cumulative Analysis
Aggregates a measure progressively over time — running totals and moving averages — to understand growth or decline.

### 3. Performance Analysis
Compares a current value against a target (its own average, or the prior period) to measure success — current sales vs. average sales, or current year vs. previous year.

### 4. Part-to-Whole Analysis
Shows how an individual part performs relative to the overall total: `(Measure / Total Measure) * 100 BY Dimension`, e.g. `(Sales / Total Sales) * 100 BY Category`.

### 5. Data Segmentation
Groups data into meaningful buckets using `CASE WHEN` — e.g. Total Products by Cost Range, Total Customers by Spending Segment (VIP / Regular / New).

### 6. Reporting
Two production-style SQL views that consolidate all metrics into single, reusable objects:
- **`gold.report_customers`** — customer name, age & age group, VIP/Regular/New segment, total orders, total sales, quantity, products, lifespan, recency, AOV, and average monthly spend.
- **`gold.report_products`** — product name/category/subcategory, revenue-based segment (High-Performer / Mid-Range / Low-Performer), total orders, sales, quantity, customers, avg selling price, AOR, and average monthly revenue.

## ▶️ How to Run

1. Load `datasets/dim_customers.csv`, `datasets/dim_products.csv`, and `datasets/fact_sales.csv` into a `gold` schema in SQL Server (or adapt the schema-qualified names for your platform).
2. Run the scripts in `scripts/` in numeric order — `06_report_customers.sql` and `07_report_products.sql` create views (`gold.report_customers`, `gold.report_products`) that the earlier scripts don't depend on, so they can also be run independently.
3. Query the views or run each analysis script's `SELECT` statements directly to reproduce the result sets shown in the documentation.

> **Note:** `06_report_customers.sql` is missing a comma after `total_products` (before `lifespan`) in the column list — add it back before running, or the script will fail with a syntax error.

## 🛠️ Requirements
- SQL Server (or any dialect supporting `DATETRUNC`, `DATEDIFF`, and window functions — adjust syntax for PostgreSQL/MySQL/Snowflake as needed)
- A SQL client (Azure Data Studio, SSMS, DBeaver, etc.)

## 📸 Screenshots
Query result screenshots for every analysis are included in [`docs/images/`](./docs/images/) and walked through in the [full documentation](./docs/SQL_Advanced_Analytics_Documentation.docx).


