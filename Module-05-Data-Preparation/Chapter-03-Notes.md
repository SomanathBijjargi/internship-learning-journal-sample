# Data Preparation in DuckDB

DuckDB's SQL engine is highly efficient for data preparation and can handle large files quickly. Below is a comprehensive guide to data preparation techniques using the DuckDB CLI, mimicking real-world business data patterns. 

## 1. Creating Sample Data
Before working with messy production data, it is best to set up a controlled environment. You can generate a sample dataset to simulate common e-commerce scenarios like missing customer info, seasonal patterns, and geographic segmentation. 

```sql
duckdb sample.duckdb <<'SQL'
CREATE OR REPLACE TABLE orders AS
SELECT
  seq AS order_id,
  CASE WHEN seq % 5 = 0 THEN NULL ELSE 'Customer ' || seq END AS customer,
  date '2025-01-01' + CAST(seq % 15 AS INTEGER) AS order_date,
  CASE WHEN seq % 3 = 0 THEN 'Widget ' || seq ELSE 'Gadget ' || seq END AS product,
  round(random()*1000, 2) AS amount,
  CASE WHEN seq % 4 = 0 THEN 'EU' ELSE 'US' END AS region
FROM range(1, 50) tbl(seq);
SQL
```

To practice handling corrupted CSV files (like unescaped quotes or missing delimiters), you can simulate a messy file.

```bash
cat <<'EOF' > messy_orders.csv
order_id,customer,order_date,product,amount,region
1,Customer 1,2025-01-01,Widget 1,100,US
2,Customer 2,2025-01-02,Gadget 2,200,US
3,Customer 3,2025-01-03,Gadget 3,300,EU
EOF
```

You can also create a large dataset to practice memory-efficient chunk processing for files too large to fit in RAM.

```sql
duckdb sample.duckdb <<'SQL'
COPY (SELECT seq AS id, random() AS val FROM range(100000)) TO 'big.csv';
SQL
```

## 2. Exploratory Data Analysis (EDA)
Quick exploratory analysis helps you understand the data structure and quality, preventing costly mistakes caused by missing values or incorrect data types.

```sql
-- Preview and get stats
SELECT * FROM orders LIMIT 5;
DESCRIBE orders;
SELECT COUNT(*) AS n, AVG(amount) AS avg_amount FROM orders;
```

## 3. Data Ingestion and Format Conversion
Converting data formats ensures your cleaned data can reach every stakeholder in their preferred format, whether it's Parquet for analytics, JSON for APIs, or CSV for Excel.

```sql
COPY (SELECT * FROM orders) TO 'orders.json' (FORMAT JSON);
COPY (SELECT * FROM orders) TO 'orders.parquet' (FORMAT PARQUET);
```

When dealing with real-world, malformed CSV files, you can instruct DuckDB to ignore errors so your pipeline doesn't break.

```sql
-- Skip bad lines while loading
SELECT * FROM read_csv_auto('messy_orders.csv', ignore_errors = true);
```

You can also seamlessly combine data from various formats (CSV, JSON, Parquet) without maintaining separate processing pipelines.

```sql
CREATE TABLE json_orders AS SELECT * FROM read_json_auto('orders.json');
CREATE TABLE parquet_orders AS SELECT * FROM read_parquet('orders.parquet');

SELECT * FROM orders
UNION ALL
SELECT * FROM parquet_orders;
```

## 4. Data Cleaning
### Handling Missing Values
Rather than excluding incomplete records entirely, strategic imputation preserves data for analysis.

```sql
-- Replace null customer names
SELECT COALESCE(customer, 'Unknown') AS customer FROM orders;
```

### String and Regex Operations
Standardizing text ensures accurate grouping and search functionality. You can use simple string operations or regular expressions for more complex patterns like fixing inconsistent spacing.

```sql
-- Basic string trimming and lowering
SELECT DISTINCT TRIM(LOWER(product)) AS clean_product FROM orders;

-- Regex search and replace for multiple spaces
SELECT REGEXP_REPLACE(product, '\\s+', ' ', 'g') AS tidy_product FROM orders;
```

### Date Parsing
Converting raw dates into standard formats is critical for time-based business analysis, forecasting, and filtering.

```sql
SELECT order_id, STRFTIME(order_date, '%Y-%m') AS order_month FROM orders;
```

## 5. Data Transformation and Subsetting
### Conditional Logic (Binning)
Categorizing continuous data (like exact dollar amounts) into distinct segments helps drive targeted decision-making and performance analysis.

```sql
SELECT 
  order_id,
  CASE
    WHEN amount > 700 THEN 'high'
    WHEN amount > 300 THEN 'medium'
    ELSE 'low'
  END AS price_band
FROM orders;
```

### Derived Columns
You can easily create new business metrics (like tax calculations or formatting codes) directly from existing data.

```sql
SELECT *, amount * 0.1 AS tax, UPPER(region) AS region_code FROM orders;
```

### Filtering and Dropping Columns
Efficient filtering limits analysis to relevant subsets and allows you to exclude sensitive information (like PII).

```sql
SELECT order_id, amount FROM orders WHERE region = 'US';
SELECT * EXCLUDE region FROM orders;
```

## 6. Advanced Processing
### Processing in Chunks
For massive datasets that exceed memory limits, you can process data in manageable chunks.

```sql
SELECT * FROM read_csv_auto('big.csv') LIMIT 1000 OFFSET 0;
SELECT * FROM read_csv_auto('big.csv') LIMIT 1000 OFFSET 1000;
```

### Summaries and Pivots
Aggregating and pivoting detailed transaction data into high-level insights is foundational for strategic planning and executive dashboards.

```sql
-- Aggregation
SELECT region, COUNT(*) AS n_orders, SUM(amount) AS total 
FROM orders 
GROUP BY region;

-- Pivot by region
SELECT * FROM orders 
PIVOT(COUNT(*) FOR region IN ('US', 'EU'));
```