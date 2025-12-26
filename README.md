# Ecommerce Analytics with SQL & Python

This project builds an end to end analytics pipeline for a Brazilian e commerce marketplace.  
Raw CSVs are loaded into a MySQL schema, then queried with analytical SQL to answer business questions on sales, retention, and customer behavior.

## Tech stack

- **Database**: MySQL (ecommerce schema)
- **Languages**: SQL, Python (Numpy, Pandas, Seaborn, Matplotlib)
- **Data**: customers,geolocation, orders, order_items, payments, products, sellers CSVs 

## 1. Data modelling & ETL

- Automatically infers SQL data types from Pandas dtypes and generates `CREATE TABLE` statements for each CSV (customers,geolocation, orders, order_items, payments, products, sellers).
- Cleans column names (spaces, hyphens, dots) and replaces `NaN` with `NULL` before insert.  
- Loads ~100k+ rows per table into a MySQL `ecommerce` database via `mysql.connector`. 

## 2. Core business SQL queries 

### Basic metrics

- **Customer footprint**: Listed all unique customer cities using `SELECT DISTINCT customer_city FROM customers`. 
- **Order volume 2017**: Counted `45,101` orders placed in 2017 with a `WHERE YEAR(order_purchase_timestamp)=2017`. 
- **Sales by category**: Joined `products`, `order_items`, and `payments` to compute total revenue for 74 product categories.  
- **Installment adoption**: Calculated the share of orders paid in installments using a `CASE` expression on `payment_installments`.
- **Customers by state**: Aggregated customers per `customer_state` to quantify geographic concentration.

### Intermediate analytics

- **Orders per month (2018)**: Grouped 2018 orders by month and visualized a bar chart in Seaborn. 
- **Avg products per order by city**: Common table expression to count items per order and then average per `customer_city` (top cities like *Padre Carvalho* ≈ 7 products/order). 
- **Category revenue share**: Computed revenue contribution (%) of each product category vs total payments. 
- **Price–demand correlation**: Correlated average product price with purchase count; found a weak negative correlation (~−0.106).
- **Top sellers by revenue**: Aggregated revenue by `seller_id`, ranked with `DENSE_RANK`, and plotted top performers.  

### Advanced analytics

- **Customer‑level moving average**: Window function over each customer’s order history to compute 3‑order moving average of `payment_value`.  
- **Monthly cumulative sales**: For each year, computed monthly revenue and cumulative sum using `SUM() OVER(ORDER BY year, month)`. 
- **YoY revenue growth**: Yearly totals and `LAG` window function; example: 2017 vs 2016 growth ≈ 12,112%, 2018 vs 2017 ≈ 20%.  
- **6‑month retention**: First‑order cohort CTE and second‑purchase within 6 months; baseline retention currently ≈ 0% (logic or data issue documented).
- **Top 3 customers per year**: Ranked customers by annual spend using window functions and visualized top spenders per year. 

## 3. Key learnings

- Designing a relational schema and ETL pipeline from raw CSVs into MySQL.  
- Writing analytical SQL with joins, CTEs, window functions (`LAG`, `DENSE_RANK`, moving averages), and partitions.
- Translating business questions (retention, category mix, top customers) into efficient queries and visualizations.

