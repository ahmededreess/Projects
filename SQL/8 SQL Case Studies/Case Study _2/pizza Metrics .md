#### /* pizza Metrics */

```
/*Customer_Orders cleaned */ 
With customer_orders_cleaned as ( 
      SELECT order_id , customer_id,pizza_id ,
             Case When exclusions = 'null' then null else exclusions END as exclusions,
             Case When extras in ('null','NaN') then null else extras END as extras, Cast(order_time as datetime ) as order_time
      FROM `customer-funnel-visualization.Case_Study_2.Customers_Orders` 
),

/* Runner_Orders_Cleaned*/
RR as (
      SELECT order_id , runner_id ,
             Cast(Case when pickup_time = 'null' then null else pickup_time End as datetime) as pickup_time,  
             Cast(Case when distance = 'null' then null else Replace(distance,'km','') End as FLOAT64) as distance ,
             CAST(Case when duration = 'null' then null else substring(duration,1,2)end as INT64) as duration ,
             Case when cancellation in ('null','NaN') then null else cancellation end as cancellation 
      FROM `customer-funnel-visualization.Case_Study_2.Runner_orders`  
),
```
#### /*1- How many pizzas are ordered? */ 
#### /*2- How many unique customer orders were made?

```
SELECT count(pizza_id) as No_pizzas , count(DISTINCT order_id) as NO_orders
From Customer_Orders_cleaned; 
```

#### /* 3- How many successful orders were delivered by each runner?*/ 

```
SELECT runner_id , count(order_id) as Successful_orders  
FROM RR 
Where cancellation is null
Group by runner_id ; 
```

#### /* 4- How many of each type of pizza was delivered? */
```
SELECT pizza_name, count(pizza_id) as pizzas_Delivered
FROM customer_Orders_cleaned as C 
INNER JOIN `customer-funnel-visualization.Case_Study_2.Pizza_names` as P using(pizza_id) 
INNER JOIN RR AS R  using(order_id) 
WHERE cancellation is null 
Group by pizza_name; 
```
#### /* 5- How many Vegetarian and Meatlovers were ordered by each customer? */
```
SELECT customer_id, pizza_name , count(pizza_id) as Ordered_Pizzas
FROM Customer_Orders_cleaned as s 
INNER JOIN `customer-funnel-visualization.Case_Study_2.Pizza_names` as P using(pizza_id)
Group by customer_id , pizza_name 
ORDER BY customer_id; 
```
#### /* 6- What was the maximum number of pizzas delivered in a single order? */
```
SELECT order_id, count(pizza_id) as Max_Pizzas
FROM Customer_orders_cleaned 
GROUP BY order_id
ORDER BY count(pizza_id) DESC
LIMIT 1; 
```
#### /* 7- For each customer, how many delivered pizzas had at least 1 change and how many had no changes */
```
SELECT customer_id, 
       SUM(CASE WHEN exclusions is null and extras is null then 1 else 0 END) as No_changes,
       SUM(CASE WHEN exclusions is not null or extras is not null then 1 else 0 END) as Changes
FROM Customer_orders_cleaned
GROUP BY customer_id
ORDER BY customer_id ;
```
#### /* 8- How many pizzas were delivered that had both exclusions and extras */
```
SELECT count(pizza_id) as Changed_pizzas
FROM Customer_orders_cleaned
WHERE exclusions is not null and extras is not null ; 
```
#### /* 9- What was the total volume of pizzas ordered for each hour of the day? */
```
SELECT Extract(hour from order_time) as Hour, count(pizza_id) as Pizzas_delivered
FROM Customer_orders_cleaned
GROUP BY Extract(hour from order_time); 
```
#### /* 10- What was the volume of orders for each day of the week? */ 
```
SELECT EXTRACT(Dayofweek from order_time) as Day , count(Distinct order_id) AS No_oders
FROM Customer_orders_cleaned
GROUP BY EXTRACT(Dayofweek from order_time);
```
