### Create order

```sql

-- create order

WITH new_order AS (
  INSERT INTO orders_pg_node1 (user_id, total_price, status, created_at, update_at)
  VALUES (2, 699.99 + 19.99, 1, NOW(), NOW())
  RETURNING order_id
),
insert_details AS (
  INSERT INTO order_details_pg_node1 (order_id, product_id, quantity, price_per_unit)
  SELECT order_id, product_id, quantity, price
  FROM (
    SELECT order_id, 1 AS product_id, 1 AS quantity, 699.99 AS price FROM new_order
    UNION ALL
    SELECT order_id, 2, 1, 19.99 FROM new_order
  ) AS items
)
INSERT INTO transactions_pg_node1 (order_id, supplier_id, payment_method, transaction_status, created_at)
SELECT order_id, 10, 'credit_card', 1, NOW()
FROM new_order;

```


### Calculate Revenue

```sql
INSERT INTO revenues_pg_node1 (
  supplier_id,
  transaction_id,
  total_earnings,
  platform_fee,
  date
)
SELECT
  t.supplier_id,
  NULL, -- vì tổng nhiều transaction nên để NULL hoặc chuyển sang revenue note
  SUM(o.total_price) AS total_earnings,
  ROUND(SUM(o.total_price) * 0.05, 2) AS platform_fee,
  CURRENT_DATE
FROM transactions_pg_node1 t
JOIN orders_pg_node1 o ON t.order_id = o.order_id
WHERE DATE(t.created_at) = CURRENT_DATE
GROUP BY t.supplier_id;

```
