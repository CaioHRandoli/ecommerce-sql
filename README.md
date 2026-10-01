<div align="right">
  <b>Language:</b> 
  <b>🇺🇸 English</b> |
  <a href="./README.pt-br.md">🇧🇷 Portuguese</a>
</div>

<div align="center">
  <h2>E-commerce project</h2>
</div>

PostgreSQL e-commerce project with 5 integrated tables. Includes SQL queries to answer 10 business intelligence challenges.<br>

First of all, we are going to use PostgreSQL to answer 10 business intelligence questions, therefore, let's follow the steps
below to start this project from scratch:<br>

**1. PostgreSQL connection**<br>
```sql
psql -d postgres -U your_name
```

**2. Database list**<br>
```sql
\l
```

**3. Database creation**<br>
```sql
CREATE DATABASE ecommerce;
```
**4. Database connection**
```sql
\c
```

**5. Table creation**<br>
```sql
-- 1. Customers Table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    state VARCHAR(2) NOT NULL,
    created_at DATE DEFAULT CURRENT_DATE
);

-- 2. Categories Table
CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    category_name VARCHAR(50) NOT NULL
);

-- 3. Products Table
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price NUMERIC(10, 2) NOT NULL,
    category_id INT NOT NULL,
    FOREIGN KEY (category_id) REFERENCES categories(category_id) ON DELETE RESTRICT
);

-- 4. Orders Table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL DEFAULT CURRENT_DATE,
    status VARCHAR(20) NOT NULL CHECK (status IN ('Pending', 'Completed', 'Canceled')),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id) ON DELETE CASCADE
);

-- 5. Order Items Table
CREATE TABLE order_items (
    item_id SERIAL PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    unit_price NUMERIC(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(order_id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(product_id) ON DELETE RESTRICT
);
```

**6. Populating tables**<br>
```sql
-- 1. Categories Table
INSERT INTO categories (category_name) VALUES
	('Electronics & Computers'),
	('Video Games & Consoles'),
	('Furniture & Office'),
	('Books & Media'),
	('Food & Beverages'),
	('Apparel & Fashion');

-- 2. Products Table
INSERT INTO products (product_name, price, category_id) VALUES 

	-- Category 1: Electronics & Computers
	('Gaming PC Desktop RTX 4070', 1499.99, 1),
	('Ultimate Gaming Desktop RTX 4090', 2999.00, 1),
	('Mid-Range Gaming PC Ryzen 7', 999.50, 1),
	('Portable Gaming Laptop 16-Inch', 1650.00, 1),
	('Gaming Monitor 27-Inch', 349.50, 1),
	('Wireless Ergonomic Mouse', 49.99, 1),
	('Mechanical RGB Keyboard', 89.90, 1),
	('Noise-Canceling Headphones', 199.99, 1),

	-- Category 2: Video Games & Consoles
	('PlayStation 5 Console', 499.99, 2),
	('PS5 DualSense Controller', 69.99, 2),
	('Xbox Series X Console', 499.00, 2),
	('Elden Ring Game', 59.99, 2),
	('Nintendo Switch OLED', 349.99, 2),
	('The Last of Us - PS3', 19.99, 2),
	('The Last of Us Remastered - PS4', 29.99, 2),
	('The Last of Us Part I - PS5', 69.99, 2),
	('Uncharted: Drake s Fortune - PS3', 14.99, 2),
	('Uncharted: The Nathan Drake Collection - PS4', 19.99, 2),
	('Uncharted 4: A Thief s End - PS4', 29.99, 2),
	('Uncharted: Legacy of Thieves Collection - PS5', 49.99, 2),
	('Batman: Arkham Asylum - PS3', 14.99, 2),
	('Batman: Arkham City - PS3', 14.99, 2),
	('Batman: Arkham Knight - PS4', 19.99, 2),
	('Batman: Arkham Collection - PS4', 39.99, 2),
	('Grand Theft Auto V - PS3', 19.99, 2),
	('Grand Theft Auto V - PS4', 29.99, 2),
	('Grand Theft Auto V Expanded & Enhanced - PS5', 39.99, 2),

	-- Category 3: Furniture & Office
	('Ergonomic Mesh Chair', 249.99, 3),
	('Adjustable Standing Desk', 420.00, 3),
	('Executive Leather Chair', 189.00, 3),
	('Bookshelf 5-Tier Unit', 115.50, 3),
	('LED Desk Lamp with Wireless Charger', 39.99, 3),

	-- Category 4: Books & Media
	('SQL Guide for Data Analysis', 39.99, 4),
	('Python Machine Learning Book', 49.50, 4),
	('Clean Code Architecture', 42.00, 4),
	('Sci-Fi Hardcover Novel', 24.99, 4),
	('World History Encyclopedia', 65.00, 4),

	
	-- Category 5: Food & Beverages
	('Organic Whole Coffee Beans 1kg', 24.99, 5),
	('Energy Drink 24-Pack', 44.90, 5),
	('Craft IPA Beer 6-Pack', 18.50, 5),
	('Matcha Green Tea Powder', 29.99, 5),
	('Sparkling Mineral Water 12-Pack', 15.00, 5),




	-- Category 6: Apparel & Fashion
	('100% Cotton Crewneck T-Shirt', 19.99, 6),
	('Slim Fit Denim Jeans', 59.90, 6),
	('Running Sports Shoes', 89.99, 6),
	('Waterproof Winter Jacket', 120.00, 6),
	('Casual Leather Belt', 25.00, 6);

-- 3. Customers Table
INSERT INTO customers (full_name, email, state, created_at) VALUES
	('Alice Smith', 'alice@example.com', 'NY', '2025-01-15'),
	('Bob Jones', 'bob@example.com', 'CA', '2025-03-22'),
	('Charlie Brown', 'charlie@example.com', 'TX', '2025-05-10'),
	('Diana Prince', 'diana@example.com', 'NY', '2025-06-01'),
	('Evan Wright', 'evan@example.com', 'FL', '2025-08-19');

-- 4. Orders Table
INSERT INTO orders (customer_id, order_date, status) VALUES
	(1, '2026-08-01', 'Completed'),
	(2, '2026-08-05', 'Completed'),
	(3, '2026-08-10', 'Canceled'),
	(4, '2026-08-15', 'Completed'),
	(5, '2026-08-20', 'Pending'),
	(1, '2026-09-01', 'Completed'),
	(2, '2026-09-05', 'Completed'),
	(3, '2026-09-12', 'Completed'),
	(4, '2026-09-18', 'Completed'),
	(5, '2026-09-25', 'Completed');

-- 5. Order Items Table
INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES 

	-- Order 1 (Alice - Completed): Ultimate PC Gaming Setup
	(1, 2, 1, 2999.00), -- Ultimate Gaming Desktop RTX 4090
	(1, 5, 1, 349.50),  -- Gaming Monitor 27-Inch
	(1, 7, 1, 89.90),   -- Mechanical RGB Keyboard

	-- Order 2 (Bob - Completed): Next-Gen Console & Games (PS5 Franchise Bundle)
	(2, 9, 1, 499.99),  -- PlayStation 5 Console
	(2, 10, 2, 69.99),  -- PS5 DualSense Controller
	(2, 16, 1, 69.99),  -- The Last of Us Part I - PS5
	(2, 20, 1, 49.99),  -- Uncharted: Legacy of Thieves Collection - PS5
	(2, 27, 1, 39.99),  -- Grand Theft Auto V Expanded & Enhanced - PS5

	-- Order 3 (Charlie - Canceled): Office Setup
	(3, 28, 1, 249.99), -- Ergonomic Mesh Chair
	(3, 29, 1, 420.00), -- Adjustable Standing Desk
	(3, 32, 1, 39.99),  -- LED Desk Lamp with Wireless Charger


	-- Order 4 (Diana - Completed): Books & Coffee
	(4, 33, 2, 39.99),  -- SQL Guide for Data Analysis
	(4, 34, 1, 49.50),  -- Python Machine Learning Book
	(4, 38, 3, 24.99),  -- Organic Whole Coffee Beans 1kg

	-- Order 5 (Evan - Pending): Fashion & Energy Drinks
	(5, 43, 4, 19.99),  -- 100% Cotton Crewneck T-Shirt
	(5, 39, 2, 44.90),  -- Energy Drink 24-Pack
	(5, 45, 1, 89.99),  -- Running Sports Shoes

	-- Order 6 (Alice - Completed): PS4 Franchise Classics (Batman, Uncharted, GTA)
	(6, 15, 1, 29.99),  -- The Last of Us Remastered - PS4
	(6, 19, 1, 29.99),  -- Uncharted 4: A Thief's End - PS4
	(6, 24, 1, 39.99),  -- Batman: Arkham Collection - PS4
	(6, 26, 1, 29.99),  -- Grand Theft Auto V - PS4

	-- Order 7 (Bob - Completed): PS3 Legacy Collection & Craft Beer
	(7, 14, 1, 19.99),  -- The Last of Us - PS3
	(7, 17, 1, 14.99),  -- Uncharted: Drake's Fortune - PS3
	(7, 21, 1, 14.99),  -- Batman: Arkham Asylum - PS3
	(7, 25, 1, 19.99),  -- Grand Theft Auto V - PS3
	(7, 40, 2, 18.50),  -- Craft IPA Beer 6-Pack

	-- Order 8 (Charlie - Completed): Mid-Range PC Setup
	(8, 1, 1, 1499.99), -- Gaming PC Desktop RTX 4070
	(8, 5, 1, 349.50),  -- Gaming Monitor 27-Inch
	(8, 6, 1, 49.99),   -- Wireless Ergonomic Mouse

	-- Order 9 (Diana - Pending): Books & Office Storage
	(9, 35, 1, 42.00),  -- Clean Code Architecture
	(9, 31, 1, 115.50), -- Bookshelf 5-Tier Unit

	-- Order 10 (Evan - Completed): Executive Office & Nintendo Switch
	(10, 13, 1, 349.99), -- Nintendo Switch OLED
	(10, 30, 1, 189.00), -- Executive Leather Chair
	(10, 8, 1, 199.99);  -- Noise-Canceling Headphones
```

**7. Business Questions**<br>

**Question 1 - Total Sales Revenue:** What is the total sales revenue generated from all completed orders in the e-commerce store?<br>
**Answer:**<br>
<img width="153" height="89" alt="answer1" src="https://github.com/user-attachments/assets/cadbe8e5-9ce2-473e-a934-7a82d0743052" /><br>
SQL Query:<br>
```sql
SELECT
   SUM(i.quantity * i.unit_price) AS total_revenue
FROM orders o
JOIN order_items i ON o.order_id = i.order_id
WHERE o.status = 'Completed';
```

**Question 2 - Top 5 Best-Selling Products:** What are the top 5 best-selling products by total units sold across all completed orders?<br>
**Answer:**<br>
<img width="343" height="153" alt="answer2" src="https://github.com/user-attachments/assets/be761b4e-141e-4f80-9b4d-1533caa180a2" /><br>
SQL Query:<br>
```sql
SELECT 
    p.product_id,
    p.product_name,
    SUM(i.quantity) AS total_units_sold
FROM order_items i
JOIN orders o ON i.order_id = o.order_id
JOIN products p ON i.product_id = p.product_id
WHERE o.status = 'Completed'
GROUP BY p.product_id, p.product_name
ORDER BY total_units_sold DESC
LIMIT 5;
```

**Question 3 - Revenue by Category:** What is the total sales revenue generated by each product category from completed orders, ordered by revenue in descending order?<br>
**Answer:**<br>
<img width="292" height="152" alt="answer3" src="https://github.com/user-attachments/assets/665c36a0-8d5b-4128-9ab4-51667de79834" /><br>
SQL Query:<br>
```sql
SELECT
   c.category_id,
   c.category_name,
   SUM(i.quantity * i.unit_price) AS total_revenue
FROM categories c
JOIN products p ON c.category_id = p.category_id
JOIN order_items i ON p.product_id = i.product_id
JOIN orders o ON i.order_id = o.order_id
WHERE o.status = 'Completed'
GROUP BY c.category_id, c.category_name
ORDER BY total_revenue DESC;
```

**Question 4 - Average Order Value:** What is the Average Order Value across all completed orders in the store?<br>
**Answer:**<br>
<img width="146" height="85" alt="answer4" src="https://github.com/user-attachments/assets/5389131c-9e10-4e33-a499-647139792812" /><br>
SQL Query:<br>
```sql
WITH order_totals AS (
   SELECT
      o.order_id,
      SUM(i.quantity * i.unit_price) AS total_order_value
   FROM orders o
   JOIN order_items i ON o.order_id = i.order_id
   WHERE o.status = 'Completed'
   GROUP BY o.order_id
)
SELECT
   ROUND(AVG(total_order_value), 2) AS average_order_value
FROM order_totals;
```

**Question 5 - Top Spending Customers:** Who are the top customers by total spending on completed orders, including their name, state, total orders placed, and total amount spent?<br>
**Answer:**<br>
<img width="382" height="148" alt="answer5" src="https://github.com/user-attachments/assets/b3f643c0-d222-45f4-9d17-a40bdbdb579e" /><br>
SQL Query:<br>
```sql
SELECT
   c.customer_id,
   c.full_name,
   c.state,
   COUNT(DISTINCT o.order_id) AS total_completed_orders,
   SUM(i.quantity * i.unit_price) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items i ON o.order_id = i.order_id
WHERE o.status = 'Completed'
GROUP BY c.customer_id, c.full_name, c.state
ORDER BY total_spent DESC;
```

**Question 6 - Franchise Sales Performance:** How much total sales revenue was generated individually by major gaming franchises
(The Last of Us, Uncharted, Batman, and Grand Theft Auto) across all completed orders?<br>
**Answer:**<br>
<img width="275" height="136" alt="answer6" src="https://github.com/user-attachments/assets/1b2fc3f1-f906-45b3-b97a-7be7f0c66e00" /><br>
SQL Query:<br>
```sql
SELECT
   CASE
      WHEN p.product_name LIKE 'The Last of Us%' THEN 'The Last of Us'
      WHEN p.product_name LIKE 'Uncharted%' THEN 'Uncharted'
      WHEN p.product_name LIKE 'Batman%' THEN 'Batman'
      WHEN p.product_name LIKE 'Grand Theft Auto%' OR p.product_name LIKE 'GTA%' THEN 'Grand Theft Auto'
      ELSE 'Other Games'
   END AS franchise,
   SUM(i.quantity) AS total_units_sold,
   SUM(i.quantity * i.unit_price) AS total_revenue
FROM order_items i
JOIN orders o ON i.order_id = o.order_id
JOIN products p ON i.product_id = p.product_id
WHERE o.status = 'Completed'
   AND(
      p.product_name LIKE 'The Last of Us%'
   OR p.product_name LIKE 'Uncharted%' 
   OR p.product_name LIKE 'Batman%' 
   OR p.product_name LIKE 'Grand Theft Auto%' 
   OR p.product_name LIKE 'GTA%'
)
GROUP BY franchise
ORDER BY total_revenue DESC;
```

**Question 7 - Monthly Sales Growth:** What is the monthly sales revenue and month-over-month growth percentage across all completed orders?<br>
**Answer:**<br>
<img width="455" height="99" alt="answer7" src="https://github.com/user-attachments/assets/387dfd93-e5e4-4e57-a23a-c73167063948" /><br>
SQL Query:<br>
```sql
WITH monthly_revenue AS (
    SELECT 
        DATE_TRUNC('month', o.order_date)::DATE AS sales_month,
        SUM(i.quantity * i.unit_price) AS total_revenue
    FROM orders o
    JOIN order_items i ON o.order_id = i.order_id
    WHERE o.status = 'Completed'
    GROUP BY DATE_TRUNC('month', o.order_date)::DATE
)
SELECT 
    sales_month,
    total_revenue,
    LAG(total_revenue) OVER (ORDER BY sales_month) AS previous_month_revenue,
    ROUND(
        ((total_revenue - LAG(total_revenue) OVER (ORDER BY sales_month)) 
        / LAG(total_revenue) OVER (ORDER BY sales_month)) * 100, 
        2
    ) AS mom_growth_percentage
FROM monthly_revenue
ORDER BY sales_month;
```

**Question 8 - Canceled Orders Impact:** What is the total financial value lost due to canceled orders, including the count of canceled orders and affected customers?<br>
**Answer:**<br>
<img width="392" height="89" alt="answer8" src="https://github.com/user-attachments/assets/b18da8a5-9984-4a65-a77b-7cf622408026" /><br>
SQL Query:<br>
```sql
SELECT
   COUNT(DISTINCT o.order_id) AS canceled_orders_count,
   COUNT(DISTINCT o.customer_id) AS affected_customers_count,
   SUM(i.quantity * i.unit_price) AS total_lost_revenue
FROM orders o
JOIN order_items i ON o.order_id = i.order_id
WHERE o.status = 'Canceled';
```

**Question 9 - Category Sales Share:** What is the total revenue per product category and its percentage contribution to the overall completed sales revenue?<br>
**Answer:**<br>
<img width="460" height="147" alt="answer9" src="https://github.com/user-attachments/assets/4ac0ba12-cd9b-4595-b0f8-90d124e5940e" /><br>
SQL Query:<br>
```sql
WITH category_totals AS (
    SELECT 
        c.category_id,
        c.category_name,
        SUM(i.quantity * i.unit_price) AS category_revenue
    FROM categories c
    JOIN products p ON c.category_id = p.category_id
    JOIN order_items i ON p.product_id = i.product_id
    JOIN orders o ON i.order_id = o.order_id
    WHERE o.status = 'Completed'
    GROUP BY c.category_id, c.category_name
)
SELECT 
    category_id,
    category_name,
    category_revenue,
    ROUND(
        (category_revenue / SUM(category_revenue) OVER ()) * 100, 
        2
    ) AS revenue_share_percentage
FROM category_totals
ORDER BY category_revenue DESC;
```

**Question 10 - Customer Repeat Rate / RFM Basis:** What is the repeat customer rate for completed orders, including the count of single-purchase
vs. repeat customers, and the average number of completed orders per customer?<br>
**Answer:**<br>
<img width="818" height="78" alt="answer10" src="https://github.com/user-attachments/assets/2b3e15e3-3e01-4f65-86d0-5c4749540290" /><br>
SQL Query:<br>
```sql
WITH customer_order_counts AS (
    SELECT 
        c.customer_id,
        c.full_name,
        COUNT(DISTINCT o.order_id) AS total_orders
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id AND o.status = 'Completed'
    GROUP BY c.customer_id, c.full_name
)
SELECT 
    COUNT(customer_id) AS total_customers,
    COUNT(CASE WHEN total_orders > 1 THEN 1 END) AS repeat_customers,
    COUNT(CASE WHEN total_orders = 1 THEN 1 END) AS single_order_customers,
    COUNT(CASE WHEN total_orders = 0 THEN 1 END) AS non_purchasing_customers,
    ROUND(
        (COUNT(CASE WHEN total_orders > 1 THEN 1 END)::NUMERIC / 
        NULLIF(COUNT(CASE WHEN total_orders > 0 THEN 1 END), 0)) * 100, 
        2
    ) AS repeat_customer_rate_percentage,
    ROUND(AVG(total_orders), 2) AS avg_orders_per_customer
FROM customer_order_counts;
```








