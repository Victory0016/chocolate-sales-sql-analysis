# 🍫 Chocolate Sales Analysis SQL Project

A structured SQL analysis of a chocolate retail business using Microsoft SQL Server Management Studio (SSMS), covering sales performance, product insights, customer behaviour, and store-level revenue across multiple dimensions.

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Tools & Technologies](#tools--technologies)
3. [Database Setup](#database-setup)
4. [Dataset Description](#dataset-description)
5. [Business Questions & SQL Solutions](#business-questions--sql-solutions)
   - [Section A: Exploratory Queries (Q1–Q10)](#section-a-exploratory-queries-q1q10)
   - [Section B: Multi-Table Analysis (Q11–Q20)](#section-b-multi-table-analysis-q11q20)
6. [Key Insights & Findings](#key-insights--findings)
7. [Skills Demonstrated](#skills-demonstrated)
8. [Project Structure](#project-structure)
9. [How to Reproduce](#how-to-reproduce)
10. [Author](#author)

---

## Project Overview

This project simulates a real-world business intelligence scenario for a chocolate retail company. As the analyst, I was tasked with answering 20 business questions using SQL ranging from basic data exploration to multi-table joins, aggregations, and subqueries.

The goal was to uncover actionable insights about:

- Revenue and profit trends
- Top-performing products, brands, and stores
- Customer segmentation by loyalty status and gender
- Geographic performance across cities and countries

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| Microsoft SQL Server Management Studio (SSMS) | Query execution and database management |
| SQL Server (T-SQL) | Query language |
| CSV Files | Raw data source (5 tables) |
| GitHub | Version control and project documentation |

---

## Database Setup

**Step 1 — Created a new database in SSMS:**

```sql
CREATE DATABASE ChocolateSales;
```

**Step 2 — Imported 5 CSV files as tables using the SSMS Import Wizard:**

The following tables were loaded into the database:

- `sales`
- `products`
- `customers`
- `stores`
- `calendar`

**Step 3 — Verified the data loaded correctly:**

```sql
SELECT * FROM sales;
SELECT * FROM products;
SELECT * FROM customers;
SELECT * FROM stores;
SELECT * FROM calendar;
```

---

## Dataset Description

| Table | Description |
|-------|-------------|
| `sales` | Transactional data including order ID, revenue, profit, quantity, discount, unit price, and foreign keys linking to customers, products, and stores |
| `products` | Product catalogue with product name, brand, category, and cocoa percentage |
| `customers` | Customer records including gender and loyalty membership status |
| `stores` | Store details including store name, city, country, and store type |
| `calendar` | Date dimension table for time-based analysis |

---

## Business Questions & SQL Solutions

### Section A: Exploratory Queries (Q1–Q10)


These queries analyse each table independently using `SELECT`, `COUNT`, `SUM`, `MIN`, `MAX`, `DISTINCT`, and `WHERE` to establish baseline business metrics.

---

#### Q1: How many total orders are recorded in the sales table?

```sql
SELECT COUNT(*) AS Total_Orders
FROM sales;
```

**Result:** `1,000,000 orders`

**Insight:** The dataset contains 1 million transactions — a large-scale dataset reflecting a high-volume retail operation.

---

#### Q2: List all unique product categories.

```sql
SELECT DISTINCT category
FROM products;
```

**Result:**

| category |
|----------|
| Truffle |
| Praline |
| White |
| Dark |
| Milk |

**Insight:** The business carries 5 distinct chocolate categories, offering a diverse product portfolio across premium and everyday segments.

---

#### Q3: How many customers are registered in the dataset?

```sql
SELECT COUNT(*) AS Total_Customers
FROM customers;
```

**Result:** `50,000 customers`

**Insight:** A substantial customer base of 50,000 registered users supports meaningful segmentation analysis by gender, age, and loyalty status.

---

#### Q4: List all stores and the cities they are located in.

```sql
SELECT
    store_name,
    city
FROM stores;
```

**Result (sample):**

| store_name | city |
|------------|------|
| Chocolate Store 1 | New York |
| Chocolate Store 2 | Melbourne |
| Chocolate Store 3 | Berlin |
| ... | ... |

**Insight:** 100 stores spread across 7 global cities, enabling geographic performance comparison across North America, Europe, and Australia.

---

#### Q5: Which products have a cocoa percentage greater than 70%?

```sql
SELECT
    product_name,
    cocoa_percent
FROM products
WHERE cocoa_percent > 70;
```

**Result:** `76 products` have a cocoa percentage above 70% (at 80% or 90% cocoa levels).

**Insight:** Over a third of the product catalogue qualifies as premium dark/high-cocoa chocolate — a segment increasingly driven by health-conscious consumers.

---

#### Q6: How many products does each brand offer?

```sql
SELECT
    brand,
    COUNT(*) AS Product_Count
FROM products
GROUP BY brand
ORDER BY Product_Count DESC;
```

**Result:**

| brand | Product_Count |
|-------|--------------|
| Cadbury | 37 |
| Ferrero | 37 |
| Lindt | 35 |
| Mars | 33 |
| Godiva | 30 |
| Hershey | 28 |

**Insight:** Cadbury and Ferrero lead portfolio size with 37 products each, while Hershey carries the smallest range. A broader portfolio often correlates with greater shelf presence and cross-sell opportunities.

---

#### Q7: Find all sales where a discount was applied.

```sql
SELECT
    order_id,
    discount
FROM sales
WHERE discount > 0;
```

**Result:** `374,872 orders` included a discount — roughly **37.5% of all transactions**.

**Insight:** More than a third of orders involved discounting, which warrants a deeper investigation into the impact on profit margins and whether discounts are driving incremental volume.

---

#### Q8: What is the total revenue generated across all sales?

```sql
SELECT
    SUM(revenue) AS Total_Revenue
FROM sales;
```

**Result:** `$25,486,128.86`

**Insight:** The business generated over **$25.4 million** in revenue across the two-year period — a healthy top-line figure for a multi-channel retail operation.

---

#### Q9: What is the total profit made from all orders?

```sql
SELECT
    SUM(profit) AS Total_Profit
FROM sales;
```

**Result:** `$10,194,564.63`

**Insight:** Total profit of **$10.2 million** represents an overall profit margin of approximately **40%** — indicating strong margin management across the product catalogue.

---

#### Q10: What is the minimum and maximum profit from a single order?

```sql
SELECT
    MIN(profit) AS Minimum_Profit,
    MAX(profit) AS Maximum_Profit
FROM sales;
```

**Result:**

| Minimum_Profit | Maximum_Profit |
|---------------|---------------|
| $0.73 | $37.43 |

**Insight:** All orders are profitable (minimum profit > $0), suggesting no loss-making transactions exist. The wide range ($0.73 to $37.43) reflects variation in order size and discount depth.

---

### Section B: Multi-Table Analysis — Q11 to Q20

These queries use `JOIN`, `GROUP BY`, `ORDER BY`, `TOP`, `CASE WHEN`, `AVG`, and subqueries to answer deeper cross-table analytical questions.

---

#### Q11: Total quantity sold per product category

```sql
SELECT
    p.category,
    SUM(s.quantity) AS Total_Quantity_Sold
FROM sales s
JOIN products p
    ON s.product_id = p.product_id
GROUP BY p.category
ORDER BY Total_Quantity_Sold DESC;
```

**Result:**

| category | Total_Quantity_Sold |
|----------|-------------------|
| Praline | 784,435 |
| White | 714,967 |
| Dark | 623,247 |
| Truffle | 462,202 |
| Milk | 385,619 |

**Insight:** Praline is the highest-volume category, outselling Milk chocolate by more than 2:1. This suggests strong consumer preference for premium filled chocolates over traditional milk variants.

---

#### Q12: Store name, city, and total revenue for each store

```sql
SELECT
    st.store_name,
    st.city,
    SUM(s.revenue) AS Total_Revenue
FROM sales s
JOIN stores st
    ON s.store_id = st.store_id
GROUP BY st.store_name, st.city
ORDER BY Total_Revenue DESC;
```

**Result (Top 5 stores):**

| store_name | city | Total_Revenue |
|------------|------|--------------|
| Chocolate Store 74 | Sydney | $261,393.77 |
| Chocolate Store 33 | Toronto | $260,672.37 |
| Chocolate Store 50 | New York | $259,526.62 |
| Chocolate Store 85 | Melbourne | $259,512.15 |
| Chocolate Store 98 | New York | $259,055.13 |

**Insight:** Sydney and Toronto produce the top revenue-generating individual stores. Multiple New York stores feature in the top tier, reflecting the city's high transaction density.

---

#### Q13: Which brand generates the highest total profit?

```sql
SELECT TOP 1
    p.brand,
    SUM(s.profit) AS Total_Profit
FROM sales s
JOIN products p
    ON s.product_id = p.product_id
GROUP BY p.brand
ORDER BY Total_Profit DESC;
```

**Result:**

| brand | Total_Profit |
|-------|-------------|
| Ferrero | $1,876,268.09 |

**Insight:** Ferrero leads all brands in profit generation with **$1.87 million**, likely driven by a combination of premium pricing and high sales volume across its 37-product portfolio.

---

#### Q14: Which country generates the most total revenue?

```sql
SELECT TOP 1
    st.country,
    SUM(s.revenue) AS Total_Revenue
FROM sales s
JOIN stores st
    ON s.store_id = st.store_id
GROUP BY st.country
ORDER BY Total_Revenue DESC;
```

**Result:**

| country | Total_Revenue |
|---------|--------------|
| Canada | $5,085,319.05 |

**Insight:** Canada is the top-performing market with over **$5 million in revenue**, making it the primary geographic focus for growth investment and marketing spend.

---

#### Q15: Revenue generated by loyalty members vs non-loyalty members

```sql
SELECT
    CASE
        WHEN c.loyalty_member = 1 THEN 'Loyalty Member'
        ELSE 'Non-Loyalty Member'
    END AS Customer_Type,
    SUM(s.revenue) AS Total_Revenue
FROM sales s
JOIN customers c
    ON s.customer_id = c.customer_id
GROUP BY c.loyalty_member;
```

**Result:**

| Customer_Type | Total_Revenue |
|---------------|--------------|
| Loyalty Member | $12,773,040.47 |
| Non-Loyalty Member | $12,713,088.39 |

**Insight:** Loyalty members generate slightly more revenue (**$12.77M vs $12.71M**) — a near-equal split suggesting the loyalty programme retains existing spend rather than significantly increasing it. This warrants a review of programme incentives to drive greater incremental revenue.

---

#### Q16: Top 5 products by total quantity sold

```sql
SELECT TOP 5
    p.product_name,
    p.brand,
    SUM(s.quantity) AS Total_Quantity_Sold
FROM sales s
JOIN products p
    ON s.product_id = p.product_id
GROUP BY p.product_name, p.brand
ORDER BY Total_Quantity_Sold DESC;
```

**Result:**

| product_name | brand | Total_Quantity_Sold |
|-------------|-------|-------------------|
| White Chocolate 60% | Mars | 74,066 |
| Praline Chocolate 50% | Ferrero | 73,940 |
| Truffle Chocolate 50% | Ferrero | 60,608 |
| Dark Chocolate 50% | Cadbury | 60,240 |
| Dark Chocolate 60% | Cadbury | 60,159 |

**Insight:** Mars's White Chocolate 60% is the single bestselling SKU. Ferrero occupies two of the top 5 positions, reinforcing its strong commercial performance. Mid-range cocoa products (50–60%) dominate volume, suggesting the sweet spot for consumer demand.

---

#### Q17: Which store type generates the most revenue?

```sql
SELECT
    st.store_type,
    SUM(s.revenue) AS Total_Revenue
FROM sales s
JOIN stores st
    ON s.store_id = st.store_id
GROUP BY st.store_type
ORDER BY Total_Revenue DESC;
```

**Result:**

| store_type | Total_Revenue |
|------------|--------------|
| Airport | $7,613,875.92 |
| Mall | $6,628,228.67 |
| Online | $6,386,035.76 |
| Retail | $4,857,988.51 |

**Insight:** Airport stores are the highest-revenue channel, generating **$7.6 million** — nearly 60% more than traditional Retail. This reflects high impulse purchasing and premium gifting behaviour in travel environments. Retail stores underperform all channels, suggesting a need for footfall or conversion strategy review.

---

#### Q18: Average order revenue for male vs female customers

```sql
SELECT
    c.gender,
    AVG(s.revenue) AS Average_Order_Revenue
FROM sales s
JOIN customers c
    ON s.customer_id = c.customer_id
GROUP BY c.gender;
```

**Result:**

| gender | Average_Order_Revenue |
|--------|----------------------|
| Female | $25.48 |
| Male | $25.49 |

**Insight:** Average order revenue is virtually identical across genders ($25.48 vs $25.49), indicating no meaningful gender-based spending difference. Personalisation strategies should focus on other dimensions such as product category preference or loyalty status.

---

#### Q19: Which city has the highest number of orders placed?

```sql
SELECT TOP 5
    st.city,
    COUNT(s.order_id) AS Total_Orders
FROM sales s
JOIN stores st
    ON s.store_id = st.store_id
GROUP BY st.city
ORDER BY Total_Orders DESC;
```

**Result:**

| city | Total_Orders |
|------|-------------|
| Toronto | 180,181 |
| Paris | 159,556 |
| London | 159,353 |
| New York | 150,532 |
| Sydney | 130,293 |

**Insight:** Toronto leads all cities by order volume with **180,181 orders**, consistent with Canada's position as the top revenue market. Paris and London closely follow, indicating strong European demand.

---

#### Q20: Products with unit price greater than the average unit price (Subquery)

```sql
SELECT DISTINCT
    p.product_name,
    s.unit_price
FROM sales s
JOIN products p
    ON s.product_id = p.product_id
WHERE s.unit_price >
(
    SELECT AVG(unit_price)
    FROM sales
)
ORDER BY s.unit_price DESC;
```

**Result:** Average unit price = **$9.00** | **25 products** priced above the average (all at $15.00)

**Sample products above average price:**

| product_name | unit_price |
|-------------|------------|
| White Chocolate 80% | $15.00 |
| Praline Chocolate 50% | $15.00 |
| Dark Chocolate 50% | $15.00 |
| Truffle Chocolate 70% | $15.00 |
| Milk Chocolate 80% | $15.00 |

**Insight:** 25 out of 200 products are priced above the $9.00 average, all sitting at the $15.00 premium tier. These products span all five categories, suggesting consistent premium-tier representation across the entire range.

---

## 💡 Key Findings & Insights

|  | Finding |
|---|---------|
| 1 | **Total Revenue: $25.5M** — Strong two-year performance across a 100-store global network |
| 2 | **Total Profit: $10.2M** — ~40% profit margin indicates excellent cost and pricing management |
| 3 | **Canada is the #1 market** — $5.08M in revenue, with Toronto as the most ordered-from city |
| 4 | **Ferrero leads profitability** — $1.87M in profit, the highest among all 6 brands |
| 5 | **Airport stores outperform all channels** — $7.6M revenue vs $4.9M for Retail stores |
| 6 | **Praline is the top-selling category** — 784K units sold, 2× higher than Milk chocolate |
| 7 | **37.5% of orders include a discount** — Warrants investigation into margin impact |
| 8 | **Loyalty programme shows minimal uplift** — Members vs non-members revenue split is near 50/50 |
| 9 | **Gender spending is equal** — Average order value is $25.48 (F) vs $25.49 (M) |
| 10 | **25 premium-tier products** priced at $15.00, all above the $9.00 catalogue average |

---

## 🧠 Skills Demonstrated

- ✅ Database creation and structured CSV data import in SSMS
- ✅ Single-table queries with `SELECT`, `WHERE`, `DISTINCT`, `GROUP BY`, `ORDER BY`
- ✅ Aggregate functions: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`
- ✅ Multi-table analysis using `INNER JOIN` across 3–4 tables
- ✅ Conditional logic using `CASE WHEN ... END`
- ✅ Top-N filtering with `TOP`
- ✅ Scalar subqueries for comparative benchmarking
- ✅ Business interpretation of query results into actionable insights

---

## 📁 Project Structure

```
chocolate-sales-sql/
│
├── README.md                        ← Full project documentation (this file)
│
├── data/
│   ├── sales.csv                    ← 1,000,000 transaction records
│   ├── products.csv                 ← 200 products across 5 categories and 6 brands
│   ├── customers.csv                ← 50,000 customer records
│   ├── stores.csv                   ← 100 stores across 6 countries
│   └── calendar.csv                 ← Date dimension table
│
└── queries/
    ├── 01_exploratory_queries.sql   ← Q1 to Q10: Single-table analysis
    └── 02_advanced_queries.sql      ← Q11 to Q20: Joins, aggregations, subqueries

---

## Skills Demonstrated

- Database creation and CSV data import in SSMS
- Writing `SELECT`, `WHERE`, `GROUP BY`, `ORDER BY`, `DISTINCT` queries
- Aggregate functions: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`
- Multi-table analysis using `INNER JOIN`
- Conditional logic with `CASE WHEN`
- Top-N filtering with `TOP`
- Scalar subqueries for comparative filtering
- Structured query organisation and documentation

---


---

## How to Reproduce

1. Install [SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) and [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms).
2. Open SSMS and connect to your local server instance.
3. Create a new database:
   ```sql
   CREATE DATABASE ChocolateSales;
   ```
4. Use the **Import Flat File** wizard (or **Import Data** task) to load each CSV from the `data/` folder into its respective table.
5. Open the SQL files in the `queries/` folder and execute them against the `ChocolateSales` database.

---

## Author

**[Victory Nnajekwu]**
Data Analyst | SQL · Excel · Power BI

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/nnajekwu-victory-2b76a234b)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat&logo=github)](https://github.com/Victory0016/Victory0016)

---

*If you found this project helpful, feel free to ⭐ star the repo or connect on LinkedIn*
