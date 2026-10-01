<div align="right">
  <b>Idioma:</b> 
  <a href="./README.md">🇺🇸 Inglês</a> |
  <b>🇧🇷 Português</b>
</div>

<div align="center">
  <h2>Projeto de E-commerce</h2>
</div>

Projeto de e-commerce em PostgreSQL com 5 tabelas integradas. Inclui consultas SQL para responder a 10 desafios de business intelligence.<br>

Primeiramente, vamos usar o PostgreSQL para responder a 10 perguntas de business intelligence, portanto, vamos seguir os passos
abaixo para iniciar este projeto do zero:<br>

**1. Conexão com banco de dados**<br>
```sql
psql -d postgres -U seu_nome
```

**2. Listar bancos de dados**<br>
```sql
\l
```

**3. Criar banco de dados**<br>
```sql
CREATE DATABASE ecommerce;
```

**4. Criar tabela**<br>
```sql
-- 1. Tabela de clientes
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    state VARCHAR(2) NOT NULL,
    created_at DATE DEFAULT CURRENT_DATE
);

-- 2. Tabela de categorias
CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    category_name VARCHAR(50) NOT NULL
);

-- 3. Tabela de produtos
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price NUMERIC(10, 2) NOT NULL,
    category_id INT NOT NULL,
    FOREIGN KEY (category_id) REFERENCES categories(category_id) ON DELETE RESTRICT
);

-- 4. Tabela de pedidos
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL DEFAULT CURRENT_DATE,
    status VARCHAR(20) NOT NULL CHECK (status IN ('Pending', 'Completed', 'Canceled')),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id) ON DELETE CASCADE
);

-- 5. Tabela de itens no pedido
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

**5. Inserção de dados**<br>
```sql
-- 1. Tabela de categorias
INSERT INTO categories (category_name) VALUES
	('Electronics & Computers'),
	('Video Games & Consoles'),
	('Furniture & Office'),
	('Books & Media'),
	('Food & Beverages'),
	('Apparel & Fashion');

-- 2. Tabela de produtos
INSERT INTO products (product_name, price, category_id) VALUES 

	-- Categoria 1: Eletrônicos e Computadores
	('Gaming PC Desktop RTX 4070', 1499.99, 1),
	('Ultimate Gaming Desktop RTX 4090', 2999.00, 1),
	('Mid-Range Gaming PC Ryzen 7', 999.50, 1),
	('Portable Gaming Laptop 16-Inch', 1650.00, 1),
	('Gaming Monitor 27-Inch', 349.50, 1),
	('Wireless Ergonomic Mouse', 49.99, 1),
	('Mechanical RGB Keyboard', 89.90, 1),
	('Noise-Canceling Headphones', 199.99, 1),

	-- Categoria 2: Videogames e Consoles
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

	-- Categoria 3: Móveis e Escritório
	('Ergonomic Mesh Chair', 249.99, 3),
	('Adjustable Standing Desk', 420.00, 3),
	('Executive Leather Chair', 189.00, 3),
	('Bookshelf 5-Tier Unit', 115.50, 3),
	('LED Desk Lamp with Wireless Charger', 39.99, 3),

	-- Categoria 4: Livros e Mídia
	('SQL Guide for Data Analysis', 39.99, 4),
	('Python Machine Learning Book', 49.50, 4),
	('Clean Code Architecture', 42.00, 4),
	('Sci-Fi Hardcover Novel', 24.99, 4),
	('World History Encyclopedia', 65.00, 4),

	-- Categoria 5: Alimentos e Bebidas
	('Organic Whole Coffee Beans 1kg', 24.99, 5),
	('Energy Drink 24-Pack', 44.90, 5),
	('Craft IPA Beer 6-Pack', 18.50, 5),
	('Matcha Green Tea Powder', 29.99, 5),
	('Sparkling Mineral Water 12-Pack', 15.00, 5),

	-- Categoria 6: Roupas e Moda
	('100% Cotton Crewneck T-Shirt', 19.99, 6),
	('Slim Fit Denim Jeans', 59.90, 6),
	('Running Sports Shoes', 89.99, 6),
	('Waterproof Winter Jacket', 120.00, 6),
	('Casual Leather Belt', 25.00, 6);

-- 3. Tabela de clientes
INSERT INTO customers (full_name, email, state, created_at) VALUES
	('Alice Smith', 'alice@example.com', 'NY', '2025-01-15'),
	('Bob Jones', 'bob@example.com', 'CA', '2025-03-22'),
	('Charlie Brown', 'charlie@example.com', 'TX', '2025-05-10'),
	('Diana Prince', 'diana@example.com', 'NY', '2025-06-01'),
	('Evan Wright', 'evan@example.com', 'FL', '2025-08-19');

-- 4. Tabela de pedidos
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

-- 5. Tabela de itens no pedidos
INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES 

	-- Pedido 1 (Alice - Concluído): Configuração de PC Gamer Completa
	(1, 2, 1, 2999.00), -- Desktop Gamer de Alta Performance RTX 4090
	(1, 5, 1, 349.50),  -- Monitor Gamer 27 Polegadas
	(1, 7, 1, 89.90),   -- Teclado Mecânico RGB

	-- Pedido 2 (Bob - Concluído): Console de Próxima Geração e Jogos (Combo de Franquia PS5)
	(2, 9, 1, 499.99),  -- Console PlayStation 5
	(2, 10, 2, 69.99),  -- Controle DualSense PS5
	(2, 16, 1, 69.99),  -- The Last of Us Part I - PS5
	(2, 20, 1, 49.99),  -- Uncharted: Legacy of Thieves Collection - PS5
	(2, 27, 1, 39.99),  -- Grand Theft Auto V Expanded & Enhanced - PS5

	-- Pedido 3 (Charlie - Cancelado): Configuração de Escritório
	(3, 28, 1, 249.99), -- Cadeira Ergonômica de Mesh
	(3, 29, 1, 420.00), -- Mesa Digital Interativa/Escrivaninha com Regulagem de Altura
	(3, 32, 1, 39.99),  -- Luminária de Mesa LED com Carregador por Indução


	-- Pedido 4 (Diana - Concluído): Livros e Café
	(4, 33, 2, 39.99),  -- Guia de SQL para Análise de Dados
	(4, 34, 1, 49.50),  -- Livro de Python e Machine Learning
	(4, 38, 3, 24.99),  -- Café Orgânico em Grãos 1kg

	-- Pedido 5 (Evan - Pendente): Moda e Energéticos
	(5, 43, 4, 19.99),  -- Camiseta Gola Careca 100% Algodão
	(5, 39, 2, 44.90),  -- Pack com 24 Energéticos
	(5, 45, 1, 89.99),  -- Tênis de Corrida

	-- Pedido 6 (Alice - Concluído): Clássicos de Franquia PS4 (Batman, Uncharted, GTA)
	(6, 15, 1, 29.99),  -- The Last of Us Remastered - PS4
	(6, 19, 1, 29.99),  -- Uncharted 4: A Thief's End - PS4
	(6, 24, 1, 39.99),  -- Batman: Arkham Collection - PS4
	(6, 26, 1, 29.99),  -- Grand Theft Auto V - PS4

	-- Pedido 7 (Bob - Concluído): Coleção Legado PS3 e Cerveja Artesanal
	(7, 14, 1, 19.99),  -- The Last of Us - PS3
	(7, 17, 1, 14.99),  -- Uncharted: Drake's Fortune - PS3
	(7, 21, 1, 14.99),  -- Batman: Arkham Asylum - PS3
	(7, 25, 1, 19.99),  -- Grand Theft Auto V - PS3
	(7, 40, 2, 18.50),  -- Pack com 6 Cervejas Artesanais IPA

	-- Pedido 8 (Charlie - Concluído): Configuração de PC Intermediário
	(8, 1, 1, 1499.99), -- PC Gamer RTX 4070
	(8, 5, 1, 349.50),  -- Monitor Gamer 27 Polegadas
	(8, 6, 1, 49.99),   -- Mouse Ergonômico Sem Fio

	-- Pedido 9 (Diana - Pendente): Livros e Organizadores de Escritório
	(9, 35, 1, 42.00),  -- Código Limpo / Arquitetura Limpa
	(9, 31, 1, 115.50), -- Estante de Livros com 5 Prateleiras

	-- Pedido 10 (Evan - Concluído): Escritório Executivo e Nintendo Switch
	(10, 13, 1, 349.99), -- Nintendo Switch OLED
	(10, 30, 1, 189.00), -- Cadeira Presidencial de Couro
	(10, 8, 1, 199.99);  -- Fone de Ouvido com Cancelamento de Ruído
```

**6. Perguntas de negócio**<br>

**Pergunta 1 - Faturamento Total de Vendas:** Qual é o faturamento total de vendas gerado a partir de todos os pedidos concluídos na loja de e-commerce?<br>
**Resposta:**<br>
<img width="153" height="89" alt="answer1" src="https://github.com/user-attachments/assets/cadbe8e5-9ce2-473e-a934-7a82d0743052" /><br>
Consulta SQL:<br>
```sql
SELECT
   SUM(i.quantity * i.unit_price) AS total_revenue
FROM orders o
JOIN order_items i ON o.order_id = i.order_id
WHERE o.status = 'Completed';
```

**Pergunta 2 - Top 5 Produtos Mais Vendidos:** Quais são os 5 produtos mais vendidos por total de unidades vendidas em todos os pedidos concluídos?<br>
**Resposta:**<br>
<img width="343" height="153" alt="answer2" src="https://github.com/user-attachments/assets/be761b4e-141e-4f80-9b4d-1533caa180a2" /><br>
Consulta SQL:<br>
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

**Pergunta 3 - Faturamento por Categoria:** Qual é o faturamento total de vendas gerado por cada categoria de produto a partir dos pedidos concluídos, ordenado pela receita em ordem decrescente?<br>
**Resposta:**<br>
<img width="292" height="152" alt="answer3" src="https://github.com/user-attachments/assets/665c36a0-8d5b-4128-9ab4-51667de79834" /><br>
Consulta SQL:<br>
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

**Pergunta 4 - Ticket Médio por Pedido:** Qual é o Ticket Médio por Pedido entre todos os pedidos concluídos na loja?<br>
**Resposta:**<br>
<img width="146" height="85" alt="answer4" src="https://github.com/user-attachments/assets/5389131c-9e10-4e33-a499-647139792812" /><br>
Consulta SQL:<br>
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

**Pergunta 5 - Clientes que Mais Gastaram:** Quem são os principais clientes por total de gastos em pedidos concluídos, incluindo nome, estado, total de pedidos realizados e o valor total gasto?<br>
**Resposta:**<br>
<img width="382" height="148" alt="answer5" src="https://github.com/user-attachments/assets/b3f643c0-d222-45f4-9d17-a40bdbdb579e" /><br>
Consulta SQL:<br>
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

**Pergunta 6 - Desempenho de Vendas por Franquia:** Qual foi o faturamento total de vendas gerado individualmente
pelas principais franquias de jogos (The Last of Us, Uncharted, Batman e Grand Theft Auto) em todos os pedidos concluídos?<br>
**Resposta:**<br>
<img width="275" height="136" alt="answer6" src="https://github.com/user-attachments/assets/1b2fc3f1-f906-45b3-b97a-7be7f0c66e00" /><br>
Consulta SQL:<br>
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

**Pergunta 7 - Crescimento Mensal de Vendas:** Qual é o faturamento mensal de vendas e o percentual de crescimento mês a mês em todos os pedidos concluídos?<br>
**Resposta:**<br>
<img width="455" height="99" alt="answer7" src="https://github.com/user-attachments/assets/387dfd93-e5e4-4e57-a23a-c73167063948" /><br>
Consulta SQL:<br>
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

**Pergunta 8 - Impacto dos Pedidos Cancelados:** Qual é o valor financeiro total perdido devido a pedidos cancelados, incluindo a contagem de pedidos cancelados e os clientes afetados?<br>
**Resposta:**<br>
<img width="392" height="89" alt="answer8" src="https://github.com/user-attachments/assets/b18da8a5-9984-4a65-a77b-7cf622408026" /><br>
Consulta SQL:<br>
```sql
SELECT
   COUNT(DISTINCT o.order_id) AS canceled_orders_count,
   COUNT(DISTINCT o.customer_id) AS affected_customers_count,
   SUM(i.quantity * i.unit_price) AS total_lost_revenue
FROM orders o
JOIN order_items i ON o.order_id = i.order_id
WHERE o.status = 'Canceled';
```

**Pergunta 9 - Participação de Vendas por Categoria:** Qual é o faturamento total por categoria de produto e sua contribuição percentual em relação ao faturamento total de vendas concluídas?<br>
**Resposta:**<br>
<img width="460" height="147" alt="answer9" src="https://github.com/user-attachments/assets/4ac0ba12-cd9b-4595-b0f8-90d124e5940e" /><br>
Consulta SQL:<br>
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

**Pergunta 10 - Taxa de Retenção de Clientes / Base RFM:** Qual é a taxa de clientes recorrentes para pedidos concluídos, incluindo
a contagem de clientes com uma única compra versus clientes recorrentes, e a média de pedidos concluídos por cliente?<br>
**Resposta:**<br>
<img width="818" height="78" alt="answer10" src="https://github.com/user-attachments/assets/2b3e15e3-3e01-4f65-86d0-5c4749540290" /><br>
Consulta SQL:<br>
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
