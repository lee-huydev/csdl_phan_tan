## SQL create foreign db

### Create extension FWD
```sql
CREATE EXTENSION IF NOT EXISTS postgres_fdw;

DROP SERVER IF EXISTS csdl_phan_tan_node1 CASCADE;

```
### Create table foreign
```sql
-- node 1

CREATE SERVER csdl_phan_tan_node1
FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (host '172.31.0.4', port '5432', dbname 'csdl_phan_tan');



CREATE USER MAPPING FOR CURRENT_USER
SERVER csdl_phan_tan_node1
OPTIONS (user 'postgres', password 'adminpassword');


SELECT umid, srvname, usename, umoptions
FROM pg_user_mappings
WHERE srvname = 'csdl_phan_tan_node1' AND usename = CURRENT_USER;


CREATE FOREIGN TABLE users_pg_node1 (
  user_id SERIAL,
  email VARCHAR(100) NOT NULL,
  password VARCHAR(150) NOT NULL
)
SERVER csdl_phan_tan_node1
OPTIONS (table_name 'users');


-- FOREIGN TABLE: user_detail_pg_node1
CREATE FOREIGN TABLE user_detail_pg_node1 (
  user_detail_id SERIAL,
  user_id INTEGER,
  name VARCHAR,
  phone_number VARCHAR(50),
  address VARCHAR(150),
  created_at TIMESTAMP,
  update_at TIMESTAMP
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'user_detail');

-- FOREIGN TABLE: category_pg_node1
CREATE FOREIGN TABLE category_pg_node1 (
  category_id SERIAL,
  category_name VARCHAR(150),
  created_at TIMESTAMP,
  update_at TIMESTAMP
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'category');

-- FOREIGN TABLE: products_pg_node1
CREATE FOREIGN TABLE products_pg_node1 (
  product_id SERIAL,
  category_id INTEGER,
  created_at TIMESTAMP,
  update_at TIMESTAMP
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'products');

-- FOREIGN TABLE: product_detail_pg_node1
CREATE FOREIGN TABLE product_detail_pg_node1 (
  product_detail_id SERIAL,
  product_id INTEGER,
  product_name VARCHAR(100),
  description VARCHAR(150),
  price DECIMAL(10,2),
  stock VARCHAR(100),
  image_url VARCHAR(200),
  weight DOUBLE PRECISION,
  dimensions VARCHAR(150),
  status BOOLEAN,
  created_at TIMESTAMP,
  update_at TIMESTAMP
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'product_detail');

-- FOREIGN TABLE: product_category_pg_node1
CREATE FOREIGN TABLE product_category_pg_node1 (
  product_id INTEGER,
  category_id INTEGER
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'product_category');

-- FOREIGN TABLE: orders_pg_node1
CREATE FOREIGN TABLE orders_pg_node1 (
  order_id SERIAL,
  user_id INTEGER,
  total_price DECIMAL(10,2),
  status INTEGER,
  created_at TIMESTAMP,
  update_at TIMESTAMP
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'orders');

-- FOREIGN TABLE: order_details_pg_node1
CREATE FOREIGN TABLE order_details_pg_node1 (
  order_detail_id SERIAL,
  order_id INTEGER,
  product_id INTEGER,
  quantity INTEGER,
  price_per_unit DOUBLE PRECISION
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'order_details');

-- FOREIGN TABLE: transactions_pg_node1
CREATE FOREIGN TABLE transactions_pg_node1 (
  transaction_id SERIAL,
  order_id INTEGER,
  supplier_id INTEGER,
  payment_method VARCHAR(50),
  transaction_status INTEGER,
  created_at TIMESTAMP
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'transactions');

-- FOREIGN TABLE: revenues_pg_node1
CREATE FOREIGN TABLE revenues_pg_node1 (
  revenue_id SERIAL,
  supplier_id INTEGER,
  transaction_id INTEGER,
  total_earnings DOUBLE PRECISION,
  platform_fee DOUBLE PRECISION,
  date TIMESTAMP
)
SERVER csdl_phan_tan_node1
OPTIONS (schema_name 'public', table_name 'revenues');
```
