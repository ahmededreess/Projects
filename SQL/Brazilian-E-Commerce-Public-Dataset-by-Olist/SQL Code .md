### Analyzing the customer_Location relation.

```
SELECT customer_state , count(customer_id) as No_Customers
FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.customers_dataset`
GROUP BY customer_state 
ORDER BY No_Customers DESC 
limit 10 ;
 
SELECT customer_city , count(customer_id) as No_Customers 
FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.customers_dataset`
GROUP BY customer_city 
ORDER BY No_Customers DESC
limit 10 ;

SELECT Count(Distinct customer_city) AS No_Cities, Count(Distinct customer_state) AS No_countries
FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.customers_dataset`;

SELECT ROUND(AVG(COUNT)) as average_No_cities
FROM (SELECT customer_state , count(distinct customer_city) as COUNT
      FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.customers_dataset`
      GROUP BY customer_state );
``` 

### Analyzing the prices of the orders. 

```
SELECT MIN(price) as Min_price,
       ROUND(AVG(price),2) as AVG_price ,
       MAX(price) as MAX_price,
       count(distinct order_id) as No_Orders, 
       count(DISTINCT product_id) as No_products
FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.order_items` ;
```

### the payment Analysis 

```
SELECT payment_type , count(order_id) as No_Orders , round(sum(payment_value)) as amount_payment 
FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.order_payments_dataset` 
GROUP BY payment_type 
ORDER BY amount_payment DESC ; 
```

### Customers review

```
SELECT review_score, count(review_score) as Rating 
FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.order_reviews_dataset`
WHERE review_score is not null 
GROUP BY review_score ; 
```

### delivery_preformance 
```
SELECT ROUND(AVG(date_diff(order_approved_at, order_purchase_timestamp,hour))) as AVG_Hours_1 ,
       ROUND(AVG(date_diff(order_delivered_carrier_date, order_approved_at,day))) as AVG_Days_2,
       ROUND(AVG(date_diff(order_delivered_customer_date,order_delivered_carrier_date,day))) as AVG_Days_3,
       ROUND(AVG(date_diff(order_estimated_delivery_date, order_delivered_customer_date,day))) as AVG_Days_4
FROM `customer-funnel-visualization.Olist_Brazilian_E_Commerce_Dataset.orders_dataset` 
WHERE order_approved_at is not null AND 
      order_delivered_carrier_date is not null AND 
      order_delivered_customer_date is not null ;

```
