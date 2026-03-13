# Data Analysis with Datasette



Datasette is an open-source multi-tool created by Simon Willison for exploring and publishing data. It instantly turns any SQLite database into a web interface with powerful exploration features, built-in JSON APIs, and visualization capabilities—all without requiring you to write any code upfront. It is highly effective for data journalism, analysis workflows, and sharing datasets with both technical and non-technical audiences.

## Installation and Quickstart

You can install Datasette and its companion tools using standard Python package managers like `pip` or `uv`, or via Homebrew:

```bash
pip install datasette
```

To start Datasette with a SQLite database, simply run:

```bash
datasette your_database.db
```

This will launch a web server at `http://localhost:8001`, giving you an instant interface to explore your data.

## Core Analysis Features

### The Interactive Interface
The database homepage displays all available tables, row counts, and a SQL query box. Clicking on any table name opens a view equipped with interactive features:
* **Sorting:** Sort data simply by clicking the column headers.
* **Pagination:** Easily navigate through large datasets.
* **Column Menus:** Access faceting and filtering options via the cog icon next to column headers.

### Faceting for Discovery
Faceting is a core feature for exploratory analysis. It allows you to see the distribution of values within a column. 
* By selecting "Facet by this", Datasette generates clickable counts of each category (e.g., showing how many orders are "completed", "pending", or "cancelled").
* You can apply **multiple facets** simultaneously to reveal deeper patterns in your data, such as looking at product categories across different regions.

### Filtering Data
Datasette provides robust filtering options to drill down into specific subsets of data. You can stack these filters to answer specific questions. Supported filter types include:
* Exact match
* Text contains
* Numeric comparisons (e.g., quantity > 1)
* Date ranges

## Advanced Analysis

### Running SQL Queries
While Datasette does not require SQL knowledge for basic exploration, it offers a built-in SQL editor for complex analysis. You can safely execute custom SQL queries because the database is opened in read-only mode. Just like standard tables, your query results can be faceted, filtered, and exported.

### Creating Canned Queries
If you have frequently run analyses, you can save them as named "canned queries." By defining these in a `metadata.json` file and launching Datasette with the `-m` flag, these queries will appear in the sidebar, effectively turning Datasette into a self-service analytics portal.

## Exporting and Publishing

### Exporting Data and API Access
Everything you see in the Datasette interface can be exported as raw data. 
* Links at the bottom of the page allow you to export the current view (respecting all active filters) to CSV, JSON, or newline-delimited JSON.
* Datasette includes built-in API access; you can simply append `.json` to any URL to retrieve the data programmatically.

### Publishing to the Web
Datasette includes a `publish` command that allows you to bundle your SQLite database into a Docker container and deploy it directly to the internet using serverless hosting providers like Google Cloud Run or Heroku. 

```bash
datasette publish cloudrun your_database.db
```
Once deployed, your database becomes a public API and interactive web interface for anyone to explore.

## Ecosystem Tools

### Plugins
Datasette supports custom templates, CSS, and an extensive ecosystem of plugins to enhance your analysis:
* **datasette-cluster-map:** Visualizes records with latitude and longitude data on an interactive map.
* **datasette-vega:** Allows you to plot line charts and graphs (e.g., tracking date-time data against case numbers).
* **datasette-copyable:** Generates copy-and-pasteable data formats, such as LaTeX tables.

### Data Preparation with sqlite-utils
Before analyzing data in Datasette, you can use a companion CLI tool called `sqlite-utils`. It is designed to manipulate SQLite databases, making it easy to:
* Insert large CSV files into SQLite database tables.
* Extract columns with duplicate strings into separate joined tables to clean the data.
* Enable full-text search on specific columns.

# Data Analysis with DuckDB and Parquet

## 1. What is DuckDB?

**DuckDB** is an **in-process analytical SQL database** designed for fast analytical queries.  
It is optimized for **OLAP (Online Analytical Processing)** workloads and works efficiently with large datasets.

### Key Features
- Runs **inside Python, R, or applications** (no server required).
- Optimized for **analytical queries on large datasets**.
- Uses **columnar storage**, which improves performance for analytics.
- Supports **SQL queries directly on files** like:
  - CSV
  - Parquet
  - JSON
  - Pandas DataFrames
- Very **fast compared to Pandas for large data processing**.

DuckDB is often called the **"SQLite for analytics"**.

---

# 2. What This Code Is About

This notebook demonstrates:

1. **Using Parquet as a data storage format**
2. **Comparing file formats** (CSV, JSON, SQLite, Parquet)
3. **Comparing performance of Pandas vs DuckDB**
4. **Running SQL queries directly on Parquet files**
5. **Combining Pandas and DuckDB operations**

The dataset used is:

**2015 Flights Dataset**

It contains flight information such as:
- Departure delay
- Arrival delay
- Flight distance
- Other flight metrics

The goal is to show that:

> DuckDB can perform analytical queries **much faster and with less memory** than Pandas.

---

# 3. Step-by-Step Implementation

## Step 1 – Install Required Libraries

```python
!pip install duckdb pandas sqlalchemy sqlite3
```
## Step 2 - Download Dataset
```python
!curl https://github.com/plotly/datasets/raw/master/2015_flights.parquet --output 2015_flights.parquet
```
## Step 3 – Load Data Using Pandas
```python
import pandas as pd

df = pd.read_parquet('2015_flights.parquet')
df.head()
```
## Step 4 – Compare Storage Formats
**Convert to CSV**
```python
%timeit df.to_csv('2015_flights.csv', index=False)
```
**Convert to JSON**
```python
%timeit df.to_json('2015_flights.json', orient='records')
```
**Convert to SQLite**
```python
%timeit df.to_sql('2015_flights', 'sqlite:///2015_flights.db', index=False, if_exists='replace')
```
**Save as Parquet**
```python
%timeit df.to_parquet('2015_flights.saved.parquet')
```
## Step 5 – Check File Sizes
```python
!ls -la 2015_flights.*
```
**Comparision of file size :** 
| Format  | Size       | Performance |
| ------- | ---------- | ----------- |
| CSV     | Large      | Slow        |
| JSON    | Very Large | Slow        |
| SQLite  | Medium     | Moderate    |
| Parquet | Small      | Fast        |

## Step 6 – Perform Analysis Using Pandas
Find the number of unique flight distances for each departure delay.
```python
delays = df.groupby('DEPARTURE_DELAY')['DISTANCE'].nunique()
delays[delays.index > 0]
```
**Measure Pandas Performance**
```python
%timeit df.groupby('DEPARTURE_DELAY')['DISTANCE'].nunique()
```
Typical execution time: `~5 seconds`

## Step 7 – Perform Same Query Using DuckDB
```python
import duckdb

delays = duckdb.query("""
SELECT
    DEPARTURE_DELAY,
    COUNT(DISTINCT DISTANCE)
FROM "2015_flights.parquet"
GROUP BY DEPARTURE_DELAY
ORDER BY DEPARTURE_DELAY
""").df()
```
**DuckDB is ~20–25x faster than Pandas.**

## Step 8 – Run DuckDB Using a Connection
```python
conn = duckdb.connect(database=':memory:', read_only=False)

delays = conn.execute("""
SELECT
DEPARTURE_DELAY,
COUNT(DISTINCT DISTANCE)
FROM "2015_flights.parquet"
GROUP BY DEPARTURE_DELAY
ORDER BY DEPARTURE_DELAY
""").df()
```
**Explanation**

1. Creates an in-memory DuckDB database and executes SQL queries.

**Advantages:**

  *  No external database required

  *  Very fast processing

## Step 9 – Query Pandas DataFrame Using DuckDB
DuckDB can query **Pandas DataFrames directly**.
```python
df = pd.read_parquet('2015_flights.parquet')

delays = duckdb.sql("""
SELECT
DEPARTURE_DELAY,
COUNT(DISTINCT DISTANCE)
FROM df
GROUP BY DEPARTURE_DELAY
ORDER BY DEPARTURE_DELAY
""").df()
```
DuckDB treats the Pandas DataFrame df as a SQL table.

Much faster than Pandas.

## Step 10 – Combine DuckDB and Pandas
**Example:**\
    Segment flights by distance:
```sql
SELECT
CASE
WHEN DISTANCE < 1000 THEN 'Short'
WHEN DISTANCE < 2000 THEN 'Medium'
ELSE 'Long'
END AS flight_type
```
This categorizes flights into:


| Category | Distance  |
| -------- | --------- |
| Short    | < 1000    |
| Medium   | 1000–2000 |
| Long     | > 2000    |
