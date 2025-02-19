# Zomato Analysis SQL Project

## 📌 Introduction
With the advent of technology, food delivery platforms have revolutionized the dining experience. Services like Zomato allow users to explore, order, and enjoy meals conveniently. However, understanding customer behavior, improving delivery logistics, and optimizing service operations are critical for growth. This project aims to design an analytical framework to study and improve Zomato's operational and customer engagement strategies through a robust database and reporting system.

## 🎯 Objectives
- 📊 Develop a data-driven analytics platform for food delivery services.
- 🔍 Identify patterns in customer behavior, order trends, and delivery efficiency.
- 📈 Provide actionable insights to improve restaurant partnerships and optimize delivery times.
- ⭐ Enhance the user experience by analyzing reviews, ratings, and feedback data.
- 💡 Support strategic decision-making for pricing, promotions, and regional expansion.

## 📂 Dataset Overview
The dataset consists of multiple tables that store information about restaurants, customers, deliveries, orders, and menu items:
- **🍽️ Restaurants**: Stores details like restaurant names, cuisine types, and ratings.
- **👥 Customers**: Contains information about customer names, cities, and order histories.
- **🚚 Deliveries**: Records delivery personnel, times, and statuses.
- **🛒 Orders**: Tracks order dates, amounts, and statuses.
- **📜 Menu Items**: Includes details about the items offered by each restaurant.

## 🔍 Problem Statement
- ⏳ Inconsistent delivery times leading to customer dissatisfaction.
- 📉 Lack of insights into customer preferences and order trends.
- 🚴 Inefficient allocation of delivery personnel and resources.
- 🏪 Limited understanding of restaurant performance and demand fluctuations.
- 💔 Difficulty in maintaining customer retention amidst growing competition.

## 🛠️ SQL Queries Used
Below are some key SQL queries used for data analysis:

### 1️⃣ Retrieve all cities with the number of customers in each city.
```sql
SELECT city, COUNT(*) AS customer_count
FROM Customers
GROUP BY city;
```

### 2️⃣ Find the total number of menu items available across all restaurants.
```sql
SELECT COUNT(*) AS total_menu_items
FROM Menu_Items;
```

### 3️⃣ Retrieve details of all orders where the total amount exceeds $50.
```sql
SELECT * FROM Orders
WHERE total_amount > 50;
```

### 4️⃣ List the average price of menu items for each restaurant.
```sql
SELECT restaurant_id, AVG(price) AS avg_price
FROM Menu_Items
GROUP BY restaurant_id;
```

### 5️⃣ Identify customers who have placed no orders.
```sql
SELECT * FROM Customers
WHERE customer_id NOT IN (SELECT DISTINCT customer_id FROM Orders);
```

### 6️⃣ Retrieve the total revenue for a specific restaurant (e.g., restaurant_id = 3).
```sql
SELECT SUM(total_amount) AS total_revenue
FROM Orders
WHERE restaurant_id = 3;
```

### 7️⃣ Find the top 3 most expensive menu items and their restaurant names.
```sql
SELECT mi.item_name, r.restaurant_name, mi.price
FROM Menu_Items mi
JOIN Restaurants r ON mi.restaurant_id = r.restaurant_id
ORDER BY mi.price DESC
LIMIT 3;
```

### 8️⃣ Retrieve customers who have placed at least one order.
```sql
SELECT DISTINCT customer_id, customer_name
FROM Customers
WHERE customer_id IN (SELECT customer_id FROM Orders);
```

### 9️⃣ List all menu items along with their restaurant names.
```sql
SELECT mi.item_name, r.restaurant_name
FROM Menu_Items mi
JOIN Restaurants r ON mi.restaurant_id = r.restaurant_id;
```

### 🔟 Retrieve the names of customers who have ordered from a specific restaurant (e.g., restaurant_id = 2).
```sql
SELECT DISTINCT c.customer_name
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
WHERE o.restaurant_id = 2;
```

### 1️⃣1️⃣ Find restaurants that do not have any menu items.
```sql
SELECT * FROM Restaurants
WHERE restaurant_id NOT IN (SELECT DISTINCT restaurant_id FROM Menu_Items);
```

### 1️⃣2️⃣ Retrieve the name of the restaurant with the highest total revenue.
```sql
SELECT r.restaurant_name, SUM(o.total_amount) AS total_revenue
FROM Orders o
JOIN Restaurants r ON o.restaurant_id = r.restaurant_id
GROUP BY r.restaurant_name
ORDER BY total_revenue DESC
LIMIT 1;
```

## 📊 Conclusion
This project provides an in-depth analysis and database-driven solution to optimize delivery logistics, improve customer satisfaction, and support data-driven decision-making. The insights derived from customer behavior, order trends, and feedback mechanisms will help Zomato streamline its operations, stay ahead of competitors, and foster long-term customer loyalty. By leveraging advanced analytics, this project contributes to a smarter and more efficient food delivery ecosystem.
