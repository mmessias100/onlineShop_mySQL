# onlineShop_mySQL



# Design a database for an online shop. It should include 4 tables: Customers, Products, Orders and Orderline.

CREATE DATABASE online_shop;
USE online_shop;

#2) Create the tables. Insert at least five records per table.
# Then update at least two records per table.
CREATE TABLE customers (
	customer_id INT NOT NULL AUTO_INCREMENT,
	customer_name varchar(50),
    email varchar(50),
    PRIMARY KEY (customer_id)
);

CREATE TABLE orders (
	order_id INT NOT NULL AUTO_INCREMENT,
    fk_customer_id INT,
    order_date date,
    order_quantity int,
    PRIMARY KEY (order_id),
    FOREIGN KEY (fk_customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE products (
	product_id INT NOT NULL AUTO_INCREMENT,
    product_name varchar(50),
    price dec(3,2),
    PRIMARY KEY (product_id)
);

CREATE TABLE orderline (
	orderline_id INT NOT NULL AUTO_INCREMENT,
    fk_order_id int NOT NULL,
    fk_product_id int NOT NULL,
    PRIMARY KEY (orderline_id),
    FOREIGN KEY (fk_order_id) REFERENCES orders(order_id),
    FOREIGN KEY (fk_product_id) REFERENCES products(product_id)
);

# Insert at least five records per table.

#Customers Table - 'customers'
INSERT INTO customers(customer_name, email) VALUES('Alex', 'alex1@qa.test');
INSERT INTO customers(customer_name, email) VALUES('Bridget', 'bridgethegap@post.mail');
INSERT INTO customers(customer_name, email) VALUES('Chris', 'chris@tmas.com');
INSERT INTO customers(customer_name, email) VALUES('Dennis', 'dennisthemenace@outlook.junk');
INSERT INTO customers(customer_name, email) VALUES('Eman', 'e.man@icloud.com');
SELECT * FROM customers;

#Orders Table - 'orders'
INSERT INTO orders(order_date, order_quantity) VALUES('2022-08-01', 2);
INSERT INTO orders(order_date, order_quantity) VALUES('2022-08-14', 4);
INSERT INTO orders(order_date, order_quantity) VALUES('2022-11-28', 8);
INSERT INTO orders(order_date, order_quantity) VALUES('2022-11-29', 32);
INSERT INTO orders(order_date, order_quantity) VALUES('2022-11-30', 64);
SELECT * FROM orders;

#Products Table - 'products'

# INSERT INTO products(product_name, price) VALUES('Air Jordan Retro 11', 300.00);
# I noticed I set the price datatype with DEC(size, d) - where the size was 3 and digits after decimal point 2.
# So for a price tag of 300.00 this would be out of range. So I altered my table below.
SELECT * FROM products;
ALTER TABLE products MODIFY COLUMN price dec(6,2);
INSERT INTO products(product_name, price) VALUES('Air Jordan Retro 11', 300.00);
INSERT INTO products(product_name, price) VALUES('Michael Kors Bag', 200.00);
INSERT INTO products(product_name, price) VALUES('Prada Womens Coat', 4200.00);
INSERT INTO products(product_name, price) VALUES('Plain White T-shirt', 10.00);
INSERT INTO products(product_name, price) VALUES('Zara Men Socks', 8.99);
SELECT * FROM products;


#3) Query your database (at least 10 different queries).

# Using IS NOT NULL, ORDER BY, and LIMIT, which product is the most expensive? Discard NULL Values.
SELECT * FROM products;
SELECT product_name, price
FROM products
WHERE price IS NOT NULL
ORDER BY price DESC LIMIT 1;


# Using IS NOT NULL, ORDER BY, and LIMIT, which product is the least expensive?
SELECT * FROM products;
SELECT product_name, price
FROM products
WHERE price IS NOT NULL
ORDER BY price ASC LIMIT 1;


# Using COUNT and JOIN ... ON, get the number of customers who bought products in November.
SELECT * FROM customers;
SELECT * FROM orders;
SELECT c.customer_name, c.email, o.order_date
FROM orders o
JOIN customers c ON c.customer_id=o.order_id WHERE o.order_date BETWEEN '2022-11-01' AND '2022-11-30';


# Using IS NOT NULL, COUNT and JOIN ... ON, get the number of customers who didn't buy products in November.
SELECT * FROM customers;
SELECT * FROM orders;
SELECT c.customer_name, c.email, o.order_date
FROM orders o
JOIN customers c ON c.customer_id=o.order_id WHERE o.order_date NOT BETWEEN '2022-11-01' AND '2022-11-30';


# Using a JOIN ... ON, which customer bought Air Jordan's?
SELECT * FROM customers;
SELECT * FROM products;
SELECT c.customer_name, c.email, p.product_name, p.price
FROM customers c
JOIN products p ON c.customer_id=p.product_id WHERE p.product_name='Air Jordan Retro 11';


# Using a JOIN ... ON, which customer bought a Prada Coat?
SELECT * FROM customers;
UPDATE customers SET customer_name = 'Christmas' WHERE customer_id= 3;
SELECT * FROM products;
SELECT c.customer_name, c.email, p.product_name, p.price
FROM customers c
JOIN products p ON c.customer_id=p.product_id WHERE p.product_name LIKE 'Prada%';


# Using a JOIN ... ON, which 2 customers spent the least on a product?
SELECT * FROM customers;
SELECT * FROM products;
SELECT c.customer_name, c.email, p.product_name, p.price
FROM customers c
JOIN products p ON c.customer_id=p.product_id ORDER BY p.price ASC LIMIT 2;


# Using a JOIN ... ON, which customer made the most orders?
SELECT * FROM customers;
SELECT * FROM orders;
SELECT c.customer_name, p.product_name, o.order_quantity
FROM customers c
JOIN products p ON c.customer_id=p.product_id
JOIN orders o ON c.customer_id=o.order_id
WHERE o.order_quantity IS NOT NULL ORDER BY o.order_quantity DESC;


# What was the total price of all orders?
SELECT * FROM orders;
SELECT * FROM products;
SELECT  SUM(p.price) AS Total
FROM products p
JOIN orders o ON p.product_id=o.order_id WHERE p.price IS NOT NULL;


# What were the first 3 orders placed?
SELECT * FROM orders;
SELECT * FROM products;
SELECT p.product_name, o.order_date
FROM products p
JOIN orders o ON p.product_id=o.order_id WHERE o.order_date IS NOT NULL ORDER BY o.order_date ASC LIMIT 3;
