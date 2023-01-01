## Danny's Dinner Case study 1 

####  /* 1- What is the total amount each customer spent at the restaurant? */ 

```
SELECT Customer_id, SUM(Price) AS Spent
FROM `customer-funnel-visualization.Case_Study.Sales` as S
INNER JOIN `customer-funnel-visualization.Case_Study.Menus` AS M ON S.product_id = M.product_id
GROUP BY customer_id ; 
```

#### /* 2- How many days has each customer visited the restaurant? */ 

```
SELECT Customer_id , COUNT(DISTINCT order_date) as visit_No 
FROM `customer-funnel-visualization.Case_Study.Sales`
GROUP BY customer_id; 
```

#### /* 3- What was the first item from the menu purchased by each customer?*/
```
Select customer_id , product_name, Rank
FROM (SELECT customer_id , product_id , row_number() over(partition by customer_id order by order_date) as Rank
From `customer-funnel-visualization.Case_Study.Sales`) AS S
INNER JOIN `customer-funnel-visualization.Case_Study.Menus` AS M ON S.product_id = M.product_id
Where Rank = 1; 
```
#### /* 4- What is the most purchased item on the menu and how many times was it purchased by all customers*/ 
```
SELECT product_name , count(product_id) as Most_purchased 
FROM `customer-funnel-visualization.Case_Study.Sales` AS S
INNER JOIN `customer-funnel-visualization.Case_Study.Menus` as M using(product_id)
GROUP BY product_name 
ORDER BY Most_purchased DESC
LIMIT 1 ;
```

#### /* 5- Which item was the most popular for each customer?*/ 

```
SELECT customer_id, product_name 
FROM(SELECT Customer_id ,product_id , count(product_id) AS Most_used , RANK() Over(partition by customer_id order by count(product_id) DESC) as Rank
FROM  `customer-funnel-visualization.Case_Study.Sales` 
GROUP BY Customer_id, product_id ) as S 
INNER join `customer-funnel-visualization.Case_Study.Menus` as M using(product_id) 
Where Rank = 1; 
```

#### /* 6- Which item was purchased first by the customer after they became a member?*/

```
with Temp_Tabels as ( SELECT customer_id ,product_id,order_date, row_number() over(partition by customer_id order by order_date) as Rank
                      FROM `customer-funnel-visualization.Case_Study.Sales` as s   
                      INNER JOIN `customer-funnel-visualization.Case_Study.Members` as m using(customer_id)
                      WHERE join_date > order_date ) 

Select customer_id, product_name 
FROM Temp_Tabels 
INNER JOIN `customer-funnel-visualization.Case_Study.Menus` as me  using(product_id) 
Where rank = 1; 
```

#### /* 7-Which item was purchased just before the customer became a member?*/ 
```
with Temp_Tab as ( SELECT customer_id ,product_id,order_date, row_number() over(partition by customer_id order by order_date DESC) as Rank
                   FROM `customer-funnel-visualization.Case_Study.Sales` as s   
                   INNER JOIN `customer-funnel-visualization.Case_Study.Members` as m using(customer_id)
                   WHERE join_date < order_date )

Select customer_id, product_name 
FROM Temp_Tab
INNER JOIN `customer-funnel-visualization.Case_Study.Menus` as me  using(product_id) 
Where rank = 1; 
```

#### /* 8- What is the total items and amount spent for each member before they became a member */ 
```
with Temp_T as ( 
                 SELECT customer_id ,product_id 
                 FROM `customer-funnel-visualization.Case_Study.Sales`  as s   
                 INNER JOIN `customer-funnel-visualization.Case_Study.Members` as m using(customer_id)
                 where order_date < join_date )

SELECT customer_id , count(product_id) as Total_items , sum(price) as amount_spent 
FROM Temp_T  
INNER JOIN `customer-funnel-visualization.Case_Study.Menus` as  me using(product_id)
GROUP BY customer_id ; 
```
#### /* 9- If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?*/ 
```
SELECT customer_id,SUM(case when product_name = 'sushi' then price * 10 * 2 else price * 10 END) as Points
FROM `customer-funnel-visualization.Case_Study.Menus` as m   
INNER JOIN `customer-funnel-visualization.Case_Study.Sales` as s  using(product_id)
GROUP BY customer_id; 
```
