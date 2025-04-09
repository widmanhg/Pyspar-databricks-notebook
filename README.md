# Retail Sales Project

This project analyzes retail sales data from a superstore to track various business metrics. The analysis is done using Databricks notebooks and leverages Spark SQL for data processing. The key metrics being tracked are as follows:

## Business Requirements:

1. **Total Number of Customers**: Count of distinct customers.
2. **Total Number of Orders**: Count of distinct orders.
3. **Total Sales**: Total sum of sales to date.
4. **Total Profit**: Total sum of profits.
5. **Top Sales by Country**: Sales grouped by country.
6. **Most Profitable Region and Country**: The region and country with the highest profit.
7. **Top Sales Category Products**: Products grouped by category, sorted by sales.
8. **Top 10 Sales Sub-Category Products**: Sub-categories grouped by sales, sorted by sales.
9. **Most Ordered Quantity Product**: The product with the highest quantity sold.
10. **Top Customer Based on Sales, City**: The customer and their city, based on total sales.

### Note:
- The dashboard/Notebook should be refreshed weekly and monthly for up-to-date insights.

---

## Databricks Notebook Source

```python
df = spark.table("default.superstore")
df.show()
```

---

## Time Period Selection

A widget is provided to select the time period for the analysis:

```python
dbutils.widgets.dropdown("time period", "Weekly", ["Weekly", "Monthly"])
```

## Date Calculation

The date ranges for weekly or monthly analysis are calculated dynamically based on the selected time period.

```python
from datetime import date, timedelta, datetime
from pyspark.sql.functions import *

time_period = dbutils.widgets.get("time period")
today = date.today()

today_last_year = today.replace(year=today.year - 1)

if time_period == 'Weekly':
    start_date = today_last_year - timedelta(days=today_last_year.weekday(), weeks=1) - timedelta(days=1)
    end_date = start_date + timedelta(days=6)

else:
    first = today_last_year.replace(day=1)
    end_date = first - timedelta(days=1)
    start_date = first - timedelta(days=end_date.day)
```

## Query to Get Total Number of Customers

```sql
select count(distinct customer_id) from sample where order_date between '{start_date}' and '{end_date}'
```

---

## Example SQL Queries:

1. **Total Number of Orders**:
```sql
SELECT DISTINCT order_id FROM sample WHERE order_date BETWEEN '{start_date}' AND '{end_date}'
```

2. **Total Sales and Profit**:
```sql
select sum(sales), sum(profit) from sample
```

3. **Top Sales by Country**:
```sql
select sum(sales), country from sample group by country
```

4. **Most Profitable Region and Country**:
```sql
select sum(sales), country, region from sample group by country, region order by sum(sales) desc
```

5. **Top Products by Sales**:
```sql
select sum(sales), category from sample group by category order by sum(sales) desc
```

6. **Top 10 Sales Sub-Category Products**:
```sql
select sum(sales), sub_category from sample group by sub_category order by sum(sales) desc limit 10
```

---

### Visualization Placeholder

[Insert visualization here: Graphs, Charts, or Dashboards that represent the insights from the SQL queries above]

---

### Installation and Setup

1. **Environment**: Databricks Workspace
2. **Language**: Python, Spark SQL
3. **Data Source**: Default superstore dataset
4. **Time Period**: Weekly or Monthly (can be selected via widget)

To run the project, clone the repository and open the notebook in Databricks. Make sure the `default.superstore` table is available in your Databricks environment for analysis.

---

### License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
