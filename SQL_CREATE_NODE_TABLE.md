### Create table at node

```sql
CREATE TABLE users (
  user_id SERIAL PRIMARY KEY,
  email VARCHAR(100) NOT NULL,
  password VARCHAR(150) NOT NULL
);

CREATE TABLE user_detail (
  user_detail_id SERIAL PRIMARY KEY,
  user_id INTEGER UNIQUE REFERENCES users(user_id),
  name VARCHAR,
  phone_number VARCHAR(50),
  address VARCHAR(150),
  created_at TIMESTAMP,
  update_at TIMESTAMP
);

CREATE TABLE category (
  category_id SERIAL PRIMARY KEY,
  category_name VARCHAR(150),
  created_at TIMESTAMP,
  update_at TIMESTAMP
);

CREATE TABLE products (
  product_id SERIAL PRIMARY KEY,
  category_id INTEGER REFERENCES category(category_id),
  created_at TIMESTAMP,
  update_at TIMESTAMP
);

CREATE TABLE product_detail (
  product_detail_id SERIAL PRIMARY KEY,
  product_id INTEGER UNIQUE REFERENCES products(product_id),
  product_name VARCHAR(100) NOT NULL,
  description VARCHAR(150),
  price DECIMAL(10,2) NOT NULL,
  stock VARCHAR(100),
  image_url VARCHAR(200),
  weight DOUBLE PRECISION,
  dimensions VARCHAR(150),
  status BOOLEAN,
  created_at TIMESTAMP,
  update_at TIMESTAMP
);

CREATE TABLE product_category (
  product_id INTEGER REFERENCES products(product_id),
  category_id INTEGER REFERENCES category(category_id),
  PRIMARY KEY (product_id, category_id)
);

CREATE TABLE orders (
  order_id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(user_id),
  total_price DECIMAL(10,2),
  status INTEGER,
  created_at TIMESTAMP,
  update_at TIMESTAMP
);

CREATE TABLE order_details (
  order_detail_id SERIAL PRIMARY KEY,
  order_id INTEGER REFERENCES orders(order_id),
  product_id INTEGER REFERENCES products(product_id),
  quantity INTEGER,
  price_per_unit DOUBLE PRECISION
);

CREATE TABLE transactions (
  transaction_id SERIAL PRIMARY KEY,
  order_id INTEGER REFERENCES orders(order_id),
  supplier_id INTEGER,
  payment_method VARCHAR(50),
  transaction_status INTEGER,
  created_at TIMESTAMP
);


CREATE TABLE revenues (
  revenue_id SERIAL PRIMARY KEY,
  supplier_id INTEGER,
  transaction_id INTEGER UNIQUE REFERENCES transactions(transaction_id),
  total_earnings DOUBLE PRECISION,
  platform_fee DOUBLE PRECISION,
  date TIMESTAMP
);


```
