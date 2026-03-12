# Data Analysis using Python and Pandas

This notebook demonstrates how to perform **basic data analysis using Python and Pandas** on a dataset of card transactions.

Pandas is a powerful Python library used for **data manipulation and analysis**.

Resources to learn Pandas:

- https://pandas.pydata.org/
- https://pandas.pydata.org/docs/user_guide/10min.html
- https://youtube.com/playlist?list=PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS

---

# Dataset

We analyze a **card transactions dataset** stored in **Parquet format**.

Parquet is a column-based storage format that is:

- Faster than CSV
- Smaller in size
- Efficient for analytics

---

# Step 1: Download Dataset

```python
# Download dataset
!curl -C- -o transactions.parquet "https://drive.usercontent.google.com/download?id=1XGvuFjoTwlybkw0cc9u34horMF9vMhrB"
```

---

# Step 2: Import Required Library

```python
import pandas as pd
```

Pandas is used to load, clean, and analyze tabular data.

---

# Step 3: Load Dataset

```python
df = pd.read_parquet("transactions.parquet")
```

This reads the **parquet file into a Pandas DataFrame**.

---

# Step 4: View First Few Rows

```python
df.head()
```

`head()` displays the **first 5 rows of the dataset**.

---

# Step 5: Inspect Dataset Structure

```python
df.info()
```

This shows:

- number of rows
- column names
- data types
- memory usage
- missing values

---

# Step 6: Summary Statistics

```python
df.describe()
```

This generates statistical information such as:

- count
- mean
- standard deviation
- minimum value
- maximum value
- quartiles

For all **numerical columns**.

---

# Step 7: View Column Names

```python
df.columns
```

This lists all columns present in the dataset.

---

# Step 8: Check Unique Values

Example:

```python
df["merchant"].unique()
```

This shows **all unique merchants** present in the dataset.

---

# Step 9: Count Unique Values

```python
df["merchant"].nunique()
```

This returns the **number of unique merchants**.

---

# Step 10: Value Counts

```python
df["merchant"].value_counts()
```

Shows how many transactions occurred for each merchant.

---

# Step 11: Filter Data

Example: Transactions greater than 1000

```python
df[df["amount"] > 1000]
```

This filters rows where **transaction amount is greater than 1000**.

---

# Step 12: Select Specific Columns

```python
df[["merchant", "amount"]]
```

This selects only the specified columns.

---

# Step 13: Group By Operation

Group transactions by merchant:

```python
df.groupby("merchant")["amount"].sum()
```

This calculates **total spending per merchant**.

---

# Step 14: Average Transaction Per Merchant

```python
df.groupby("merchant")["amount"].mean()
```

This calculates **average transaction amount per merchant**.

---

# Step 15: Sorting Values

```python
df.sort_values("amount", ascending=False)
```

Sorts the dataset based on **transaction amount**.

---

# Step 16: Top Transactions

```python
df.sort_values("amount", ascending=False).head()
```

Displays the **largest transactions**.

---

# Step 17: Basic Data Filtering Example

Transactions from a specific merchant:

```python
df[df["merchant"] == "Amazon"]
```

---

# Step 18: Multiple Conditions

```python
df[(df["amount"] > 500) & (df["merchant"] == "Amazon")]
```

Filters rows where:

- amount > 500
- merchant is Amazon

---

# Step 19: Save Processed Data

```python
df.to_csv("transactions_cleaned.csv", index=False)
```

Exports the DataFrame into a **CSV file**.

---


# Data Analysis with Databases

Databases are where most **transactional data** resides.  
Instead of exporting everything into CSV files, analysts often **query the database directly**.

In this notebook we demonstrate how to:

- Connect to a database
- Run SQL queries from Python
- Analyze the results using **Pandas**

---

# Query the Database

There are many ways to connect to and query a database.

In Python, a common approach is using:

- **SQLAlchemy** → database connection
- **Pandas** → executing queries and analyzing results

---

# Step 1: Database Connection

```python
stats_connection = "mysql+pymysql://guest:relational@db.relational-data.org/stats"
```

This connection string contains:

- Database type → `mysql`
- Driver → `pymysql`
- Username → `guest`
- Password → `relational`
- Database host → `db.relational-data.org`
- Database name → `stats`

---

# Step 2: Create Engine and Connect

```python
import pandas as pd
import sqlalchemy as sa

engine = sa.create_engine(
    "mysql+pymysql://guest:relational@db.relational-data.org/stats"
)

pd.read_sql("SELECT COUNT(*) FROM posts", engine)
```

This verifies that we **successfully connected to the database** and counts the rows in the `posts` table.

---

# Step 3: Helper Function for Queries

Instead of writing `pd.read_sql()` every time, we create a helper function.

```python
def query(sql):
    return pd.read_sql(sql, engine)
```

Now we can easily run SQL queries.

---

# Example Query

```python
query(
"""
SELECT
OwnerUserId,
COUNT(*) as PostCount
FROM posts
GROUP BY OwnerUserId
ORDER BY PostCount DESC
"""
)
```

This query:

1. Groups posts by user
2. Counts how many posts each user made
3. Sorts users by post count

---

# Dataset Note

The user names are `None` because the dataset is **anonymized**.

---

# How Concentrated Are the Posts?

A common question in online communities:

> Do **20% of users create 80% of the content?**

This is related to the **Pareto Principle (80/20 rule)**.

---

# Step 4: Count Posts per User

```python
post_count = query(
"""
SELECT
OwnerUserId,
COUNT(*) as PostCount
FROM posts
GROUP BY OwnerUserId
ORDER BY PostCount DESC
"""
)
```

This produces a table showing:

| User | Number of Posts |
|-----|-----|

---

# Step 5: Compare Top 20% Users

```python
post_count.PostCount.iloc[:len(post_count)//5].sum() / post_count.PostCount.sum()
```

Explanation:

- `len(post_count)//5` → top 20%
- `.sum()` → total posts by those users
- divide by total posts

Result:

> Top **20% of users generate ~76% of posts**

This is close to the **Pareto distribution**.

---

# Does Age Predict Reputation?

The `users` table contains:

- `Age`
- `Reputation`

We want to check whether **age influences reputation**.

---

# Step 6: Fetch Aggregated Statistics

```python
import numpy as np

stats_query = """
SELECT
COUNT(*) as n,
SUM(Age) as sum_age,
SUM(Reputation) as sum_rep,
SUM(Age * Reputation) as sum_age_rep,
SUM(Age * Age) as sum_age_sq,
SUM(Reputation * Reputation) as sum_rep_sq
FROM users
WHERE Age IS NOT NULL
"""

stats = query(stats_query)
stats
```

This query retrieves aggregated statistics needed to compute **correlation**.

---

# Correlation Result

The calculated correlation is about:

```
~1.7%
```

Interpretation:

> Age and reputation have **almost no relationship**.

This means older users do **not necessarily have higher reputation**.

---

# How Many Views Increase Reputation by 1 Point?

Hypothesis:

> More **profile views** may increase **user reputation**.

---

# Step 7: Fetch Required Aggregates

```python
sql = """
SELECT
COUNT(*) as n,
SUM(Views) as sum_views,
SUM(Reputation) as sum_rep,
SUM(Views * Reputation) as sum_views_rep,
SUM(Views * Views) as sum_views_sq
FROM users
"""

query(sql)
```

This gathers statistics needed to estimate the **relationship between views and reputation**.

---

# Inspect the Data Directly

To better understand the relationship:

```python
reputation = query("SELECT Views, Reputation FROM users")
reputation
```

---

# Calculate Mean Values

```python
reputation.mean()
```

This computes the **average views and reputation** across users.

---

# Interpretation

The analysis suggests that:

- Users with **more profile views tend to have higher reputation**
- However, the relationship is **not perfectly linear**

Reputation depends on many other factors like:

- Answers posted
- Question quality
- Community voting

---

# Lessons

This notebook demonstrates how to combine:

- **SQL** for querying data
- **Pandas** for analysis
- **SQLAlchemy** for database connections

Together they form a powerful workflow for **data analysis directly from databases**.

Useful libraries:

- https://pandas.pydata.org/
- https://docs.sqlalchemy.org/

---

# What Else Should You Know for Exams?

You should also know about **SQLite**.

SQLite is:

- A lightweight database
- Serverless
- Stored in a single file

Website:

https://sqlite.org/

It is widely used in:

- Mobile apps
- Embedded systems
- Small applications