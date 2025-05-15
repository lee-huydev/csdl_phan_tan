### Create table at node database

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


### Insert data sample

```sql
INSERT INTO users (email, password)
VALUES
  ('demo1@example.com', 'hashed_password_3');


INSERT INTO category (category_name, created_at, update_at)
VALUES
  ('Electronics', NOW(), NOW()),
  ('Books', NOW(), NOW()),
  ('Clothing', NOW(), NOW());



INSERT INTO products (category_id, created_at, update_at)
VALUES
  (1, NOW(), NOW()), -- thuộc Electronics
  (2, NOW(), NOW()), -- thuộc Books
  (3, NOW(), NOW()); -- thuộc Clothing


INSERT INTO product_detail (product_id, product_name, description, price, stock, image_url, weight, dimensions, status, created_at, update_at)
VALUES
  (1, 'Smartphone', 'Latest model', 699.99, 'In Stock', 'https://example.com/image1.jpg', 0.2, '15x7x0.8 cm', true, NOW(), NOW()),
  (2, 'Novel Book', 'Bestseller 2024', 19.99, 'Available', 'https://example.com/image2.jpg', 0.5, '20x13x2 cm', true, NOW(), NOW()),
  (3, 'T-Shirt', '100% Cotton', 9.99, 'Limited Stock', 'https://example.com/image3.jpg', 0.3, 'M size', true, NOW(), NOW());

```
