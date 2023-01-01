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
