 ### /* 1- How many customers has Foodie-Fi ever had ? */ 
```
   SELECT count(Distinct customer_id) as No_custoemrs 
   FROM `customer-funnel-visualization.Case_Study_3.Subscriptions`; 
```
  ### /* 2- What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value*/
```
 SELECT EXTRACT(month from start_date) as Month , count(customer_id) as No_trial_plans 
 FROM `customer-funnel-visualization.Case_Study_3.Subscriptions` 
 Group by extract(month from start_date) ;
```
###   /* 3- What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name*/ 
```
SELECT plan_name , count(plan_id) as events
FROM `customer-funnel-visualization.Case_Study_3.Plans` as P 
INNER JOIN `customer-funnel-visualization.Case_Study_3.Subscriptions` as S  using(plan_id) 
WHERE extract(year from start_date) > 2020
GROUP BY plan_name; 
```
 ###  /* 4- What is the customer count and percentage of customers who have churned rounded to 1 decimal place?*/ 
```
select count(customer_id) as No_customers,
       (count(customer_id) / (select count(distinct customer_id) from `customer-funnel-visualization.Case_Study_3.Subscriptions`))*100 as Percentage 
From `customer-funnel-visualization.Case_Study_3.Subscriptions` 
WHERE plan_id = 4 ; 
```
###   /* 5- What is the number and percentage of customer plans after their initial free trial?*/ 
```
with SS as (SELECT customer_id, plan_id, start_date , row_number() over(partition by customer_id order by start_date) as Orders
FROM `customer-funnel-visualization.Case_Study_3.Subscriptions`
WHERE plan_id <> 0)

SELECT plan_name ,count(plan_id),
      ROUND((count(plan_id) / ( select count(plan_id)
                                from SS 
                                where orders = 1))  * 100,2) as Percentage 
FROM SS  
INNER JOIN `customer-funnel-visualization.Case_Study_3.Plans` as P using(plan_id)
WHERE orders = 1
GROUP BY plan_name ;
```
 ###  /* 6- What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?*/ 
```
  With AA as (SELECT Customer_id , plan_id , row_number() over(partition by customer_id order by customer_id, start_date DESC) as Rank
  FROM `customer-funnel-visualization.Case_Study_3.Subscriptions` 
  WHERE start_date <= '2020-12-31') 

  SELECT 
    plan_name, count(customer_id) as No_cusotmers , 
    ROUND(count(customer_id) / (SELECT count(Distinct customer_id) FROM `customer-funnel-visualization.Case_Study_3.Subscriptions`) * 100,2) as Percentage 
 FROM AA 
   INNER JOIN `customer-funnel-visualization.Case_Study_3.Plans` as P using(plan_id)
   WHERE Rank = 1
   GROUP BY plan_name;
```
###   /* 7- How many customers have upgraded to an annual plan in 2020?*/ 
```
SELECT plan_id , count(customer_id) as No_customer_id 
FROM `customer-funnel-visualization.Case_Study_3.Subscriptions`
WHERE start_date <= '2020-12-31' AND plan_id = 3 
GROUP BY plan_id ; 
```
###   /* 8- How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?*/ 
```
With AD as (SELECT customer_id, plan_id , start_date as Annual_start_date
FROM `customer-funnel-visualization.Case_Study_3.Subscriptions` 
WHERE plan_id = 3 
ORDER by customer_id) 

SELECT ROUND(AVG(DATE_DIFF(AD.Annual_start_date, start_date, day)),2) as Average_days_of_Annual_plan
FROM AD 
INNER JOIN `customer-funnel-visualization.Case_Study_3.Subscriptions` as S  using(customer_id) 
WHERE S.plan_id = 0 ; 
```
###   /* 9- Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)*/

 
###   /* 10 - How many customers downgraded from a pro monthly to a basic monthly plan in 2020?*/ 
```
With AD as (SELECT customer_id, plan_id , start_date as Annual_start_date
FROM `customer-funnel-visualization.Case_Study_3.Subscriptions` 
WHERE plan_id = 3 
ORDER by customer_id) 
```
