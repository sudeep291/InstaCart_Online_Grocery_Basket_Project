# InstaCart_Online_Grocery_Basket_Project



# 🛒 Instacart Lakehouse & AI Analytics

A Medallion Lakehouse built on **Databricks** processing **33.8M rows** of Instacart data, structured with a Kimball Star Schema, and queried using **Databricks Genie AI**.

---

## 🚀 Tech Stack
* **Engine:** PySpark, Databricks, Delta Lake
* **Architecture:** Medallion (Bronze ➔ Silver ➔ Gold)
* **Modeling:** Kimball Star Schema (Multi-Grain)
* **AI/BI:** Databricks Genie (Plain English to Spark SQL)

---

## 🏗️ Architecture
1. **Bronze (Raw):** Ingested raw CSV files (~33.8M rows) into Delta format.
2. **Silver (Cleaned):** Handled missing values, cast data types, and deduplicated users.
3. **Gold (Star Schema):** 
   * `dim_products` (49.6K rows) – Enriched with aisles and departments.
   * `dim_users` (206K rows) – Unique customer IDs.
   * `fact_orders` (3.34M rows) – Order-level summaries and basket sizes.
   * `fact_order_items` (33.8M rows) – Individual cart line items.

---

## ⚡ Key Optimizations
* **Delta Z-Ordering:** Applied `ZORDER BY` on high-cardinality keys (`user_id`, `product_id`) for fast file-skipping.
* **Two Fact Tables:** Separated checkout orders (3.34M) from line items (33.8M) to reduce query scan costs by **90%** for high-level metrics.
* **Broadcast Joins:** Broadcasted small lookup tables (aisles, departments) to avoid cluster network shuffles.

---

## 🤖 AI Querying (Databricks Genie)
Connected the Gold tables directly to **Databricks Genie**, allowing stakeholders to ask business questions in natural English:
* *"What is the peak order hour of the day?"* ➔ Instant SQL scan on `fact_orders`.
* *"Top 10 reordered items in produce"* ➔ Fast join between `fact_order_items` and `dim_products`.
