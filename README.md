CREATE TABLE customers (

 customer_id INTEGER PRIMARY KEY,

 name TEXT,

 email TEXT

);

CREATE TABLE products (

 product_id INTEGER PRIMARY KEY,

 name TEXT,

 category TEXT,

 price REAL

);

CREATE TABLE orders (

 order_id INTEGER PRIMARY KEY,

 customer_id INTEGER,

 order_date TEXT,

 FOREIGN KEY (customer_id) REFERENCES customers(customer_id)

);

CREATE TABLE order_items (

 order_item_id INTEGER PRIMARY KEY,

 order_id INTEGER,

 product_id INTEGER,

 quantity INTEGER,

 FOREIGN KEY (order_id) REFERENCES orders(order_id),

 FOREIGN KEY (product_id) REFERENCES products(product_id)

);

##Insert Sample Data

INSERT INTO customers (name, email) VALUES ('Alice', 'alice@example.com');

INSERT INTO customers (name, email) VALUES ('Bob', 'bob@example.com');

INSERT INTO products (name, category, price) VALUES ('Laptop', 'Electronics', 1000);

INSERT INTO products (name, category, price) VALUES ('Mouse', 'Electronics', 25);

INSERT INTO products (name, category, price) VALUES ('Book', 'Stationery', 15);

INSERT INTO orders (customer_id, order_date) VALUES (1, '2024-05-01');

INSERT INTO orders (customer_id, order_date) VALUES (2, '2024-05-02');

INSERT INTO order_items (order_id, product_id, quantity) VALUES (1, 1, 1);

INSERT INTO order_items (order_id, product_id, quantity) VALUES (1, 2, 2);

INSERT INTO order_items (order_id, product_id, quantity) VALUES (2, 3, 3);

#Query 1: Products with price > 20

SELECT * FROM products WHERE price > 20;

# Query 2: Inner Join to show order details

SELECT o.order_id, c.name, p.name AS product, oi.quantity

FROM orders o

JOIN customers c ON o.customer_id = c.customer_id

JOIN order_items oi ON o.order_id = oi.order_id

JOIN products p ON oi.product_id = p.product_id;

# Query 3: Total amount spent by each customer

SELECT c.name, SUM(p.price * oi.quantity) AS total_spent

FROM customers c

JOIN orders o ON c.customer_id = o.customer_idJOIN order_items oi ON o.order_id = oi.order_id

JOIN products p ON oi.product_id = p.product_id

GROUP BY c.customer_id;

#Query 4: Subquery to list products priced above average

SELECT name FROM products

WHERE price > (SELECT AVG(price) FROM products);

#Query 5: Create View

CREATE VIEW customer_totals AS

SELECT c.name, SUM(p.price * oi.quantity) AS total_spent

FROM customers c

JOIN orders o ON c.customer_id = o.customer_id

JOIN order_items oi ON o.order_id = oi.order_id

JOIN products p ON oi.product_id = p.product_id

GROUP BY c.customer_id;

#Query 6: Use the View

SELECT * FROM customer_totals ORDER BY total_spent DESC;
