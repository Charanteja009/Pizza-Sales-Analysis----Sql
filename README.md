# 🍕 Pizza Sales SQL Analysis Project

![Alt Text](https://github.com/Charanteja009/Pizza-Sales-Analysis----sql/blob/326d063db5b52f1c5bbc06846b05eeae53858607/pizza.jpg)

This project contains **13 SQL questions** with solutions, written and tested using a **Pizza Sales dataset**.  
The queries demonstrate SQL skills including **JOINs, Aggregations, Window Functions, Ranking, CTEs, and Subqueries**.  

The goal of this analysis is to extract insights from pizza sales data to:

- Understand total revenue and order trends  
- Identify most popular pizza types and sizes  
- Analyze revenue distribution by category  
- Track daily and hourly order patterns  
- Determine top-performing pizzas by quantity and revenue  

These insights can help a pizza business make **data-driven decisions** regarding menu planning, pricing, and marketing.

---
 

---

## 📊 Questions & Answers
### BASIC QUESTIONS 
#### 1: Retrieve the total number of orders placed.
```sql
SELECT 
    COUNT(order_id) AS total_orders
FROM
    orders_details;
```
#### 2: Calculate the total revenue generated from pizza sales.
```sql
SELECT 
    ROUND(SUM(orders_details.quantity * pizzas.price),
            2)
FROM
    orders_details
        JOIN
    pizzas ON orders_details.pizza_id = pizzas.pizza_id;
```
#### 3: Identify the highest-priced pizza.
```sql
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```
#### 4: Identify the most common pizza size ordered.
```sql
SELECT 
    pizzas.size, COUNT(orders_details.quantity) AS total_orders
FROM
    orders_details
        JOIN
    pizzas ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY size
ORDER BY total_orders DESC;
```
#### 5: List the top 5 most ordered pizza types along with their quantities.
```sql
SELECT 
    pizza_types.name,
    SUM(orders_details.quantity) AS total_quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY name
ORDER BY total_quantity DESC
LIMIT 5;
```

### INTERMIDIATE QUESTIONS 
#### 1: Join the necessary tables to find the total quantity of each pizza category ordered.
```sql
SELECT 
    pizza_types.category,
    SUM(orders_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    orders_details ON pizzas.pizza_id = orders_details.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
```
#### 2: Determine the distribution of orders by hour of the day.
```sql
SELECT 
    HOUR(order_time) AS hour, COUNT(order_id) AS orders_count
FROM
    orders
GROUP BY hour; 
```
#### 3: Identify the highest-priced pizza.
```sql
SELECT 
    category, COUNT(name)
FROM
    pizza_types
GROUP BY category;
```
#### 4: 4.Group the orders by date and calculate the3 average number of pizzas ordered per day.
```sql
SELECT 
    ROUND(AVG(total_quan), 0) AS avg_perday
FROM
    (SELECT 
        orders.order_date,
            SUM(orders_details.quantity) AS total_quan
    FROM
        orders
    JOIN orders_details ON orders.order_id = orders_details.order_id
    GROUP BY order_date) AS order_quantity;

```
#### 5: Determine the top 3 most ordered pizza types based on revenue.
```sql
SELECT 
    pizza_types.name,
    SUM(orders_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    orders_details ON pizzas.pizza_id = orders_details.pizza_id
GROUP BY name
ORDER BY revenue DESC
LIMIT 3;
```
### ADVANCED QUESTIONS 
#### 1: Calculate the percentage contribution of each pizza type to total revenue.
```sql
SELECT 
    pizza_types.category,
    SUM(orders_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    orders_details ON pizzas.pizza_id = orders_details.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
```
#### 2:Analyze the cumulative revenue generated over time.
```sql
SELECT 
    order_date, 
    SUM(revenue) OVER(ORDER BY order_date) AS cumulative_revenue
FROM (
    SELECT 
        orders.order_date, 
        SUM(orders_details.quantity * pizzas.price) AS revenue
    FROM
        orders 
    JOIN orders_details 
        ON orders.order_id = orders_details.order_id
    JOIN pizzas 
        ON orders_details.pizza_id = pizzas.pizza_id
    GROUP BY order_date
) AS total_revenue;

```
#### 3: Determine the top 3 most ordered pizza types based on revenue for each pizza category.
```sql
SELECT 
    category, 
    name, 
    revenue, 
    ranking
FROM (
    SELECT 
        category, 
        name, 
        revenue,
        RANK() OVER(PARTITION BY category ORDER BY revenue DESC) AS ranking
    FROM (
        SELECT 
            pizza_types.category, 
            pizza_types.name,
            SUM(orders_details.quantity * pizzas.price) AS revenue
        FROM
            pizza_types 
        JOIN pizzas 
            ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN orders_details 
            ON pizzas.pizza_id = orders_details.pizza_id
        GROUP BY category, name
    ) AS table3
) AS table2
WHERE ranking <= 3;

```

## 📝 Conclusion

The **Pizza Sales SQL Analysis Project** provides valuable insights into the sales patterns and revenue generation of a pizza business. By analyzing the dataset with SQL queries, we were able to:

- **Identify total orders and overall revenue**, giving a clear picture of business performance.
- **Determine the most popular pizzas and sizes**, helping optimize inventory and menu offerings.
- **Understand sales trends by hour and date**, which can improve staffing and delivery planning.
- **Analyze revenue distribution by category and top performers**, enabling targeted marketing and promotions.
- **Track cumulative revenue over time**, highlighting growth trends and seasonality.
- **Rank pizzas by category and revenue**, supporting strategic decision-making for menu management.

Overall, this project demonstrates how **SQL analytics** can be used to turn raw sales data into actionable business insights, helping improve decision-making, maximize revenue, and enhance customer satisfaction.



