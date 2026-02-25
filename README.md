# olist-ecommerce-data-analysis
SQL + Excel analysis of Olist e-commerce dataset to extract revenue, customer behavior and payment insights.
## 📸 Dashboard Preview

![Dashboard](DASHBOARD.png)
## 🧾 SQL Analysis

You can view the complete SQL queries and analysis here:

[Download SQL Analysis](Olist_SQL_Project_Summary.docx)
### 🔍 Sample SQL Queries & Insights

#### 1️⃣ Total Revenue
```sql
SELECT ROUND(SUM(price + freight_value), 2) AS total_revenue
FROM order_items;
```
**Insight:** Calculates total platform revenue including product price and shipping, giving an overall business performance metric.

---

#### 2️⃣ Revenue by Order Status
```sql
SELECT o.order_status,
       ROUND(SUM(oi.price + oi.freight_value), 2) AS revenue
FROM orders o
JOIN order_items oi
ON o.order_id = oi.order_id
GROUP BY o.order_status
ORDER BY revenue DESC;
```
**Insight:** Delivered orders contribute the highest revenue, while cancelled/unavailable orders contribute very little, indicating strong fulfillment efficiency.

---

#### 3️⃣ Orders by Payment Type
```sql
SELECT p.payment_type,
       COUNT(o.order_id) AS total_orders
FROM payments p
JOIN orders o
ON p.order_id = o.order_id
GROUP BY p.payment_type;
```
**Insight:** Credit card is the most used payment method, showing customer preference for digital payments.

---

#### 4️⃣ Order Status by Payment Type
```sql
SELECT p.payment_type,
       o.order_status,
       COUNT(DISTINCT o.order_id) AS order_count
FROM orders o
JOIN payments p
ON o.order_id = p.order_id
GROUP BY p.payment_type, o.order_status;
```
**Insight:** Some payment types show higher cancellation rates, indicating payment method impacts order success.

---

#### 5️⃣ Revenue by State
```sql
SELECT c.customer_state,
       ROUND(SUM(oi.price + oi.freight_value), 2) AS revenue
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_state
ORDER BY revenue DESC;
```
**Insight:** Revenue is concentrated in a few states, with some regions generating higher value orders.
