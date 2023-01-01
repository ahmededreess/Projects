### first to merge all tables in one table and then use it for analysis 

```
WITH RECURSIVE merge_table as (
SELECT U.user_id , date, device , sex , H.page as Home_page , S.page as Search_page , P.page as payment_page , C.page as confirmation_page
FROM `customer-funnel-visualization.Customer_data.User_table` AS U
LEFT JOIN `customer-funnel-visualization.Customer_data.home_page` as H
ON U.user_id = H.user_id 
LEFT JOIN `customer-funnel-visualization.Customer_data.Search_table` as S  
ON  U.user_id = S.user_id 
LEFT JOIN `customer-funnel-visualization.Customer_data.Payment_table` as P 
ON U.user_id = P.user_id 
LEFT JOIN `customer-funnel-visualization.Customer_data.Confirmation_table` as C 
ON U.user_id = C.user_id
)
``` 

### is to create the conversation funnel to analyze it and check where you need to focus 

```
SELECT Home_page , count(Distinct user_id) as No_customers
FROM merge_table
WHERE Home_page IS NOT NULL
Group by Home_page 
  UNION ALL 
    SELECT Search_page , count(distinct user_id) 
    FROM merge_table  
    WHERE Search_page IS NOT NULL
    Group by Search_page 
  UNION ALL 
    SELECT payment_page , count(distinct user_id)
    FROM merge_table 
    WHERE payment_page IS NOT NULL
    Group by payment_page
  UNION ALL
    SELECT confirmation_page , count(distinct user_id)
    FROM merge_table   
    WHERE confirmation_page IS NOT NULL
    Group by confirmation_page 
ORDER by No_customers DESC 
``` 
### to check if the gender of the customer has an impact on the customer funnel

```
SELECT sex ,
        count(home_page) as Home_page ,
        count(Search_page) as Search_page,
        count(payment_page) as payment_page,
        count(confirmation_page) as confirmation_page
FROM merge_table
Group by sex ;
```

### to check the demand of the customer on which product 

```
SELECT device ,
        count(home_page) as Home_page ,
        count(Search_page) as Search_page,
        count(payment_page) as payment_page,
        count(confirmation_page) as confirmation_page
FROM merge_table
Group by device ; 
```

### to check if the relation between the type of product and the gender 
```
SELECT sex , device,
        count(home_page) as Home_page ,
        count(Search_page) as Search_page,
        count(payment_page) as payment_page,
        count(confirmation_page) as confirmation_page
FROM merge_table
Group by sex , device ;
```

### to check if the date has an impact on the purchasing process 

```
SELECT EXTRACT(MONTH FROM date) AS Month, device ,
        count(home_page) as Home_page ,
        count(Search_page) as Search_page,
        count(payment_page) as payment_page,
        count(confirmation_page) as confirmation_page
FROM merge_table
Group by device ,Month
ORDER BY Device ;

SELECT EXTRACT(MONTH FROM date) AS Month ,
        count(home_page) as Home_page ,
        count(Search_page) as Search_page,
        count(payment_page) as payment_page,
        count(confirmation_page) as confirmation_page
FROM merge_table
Group by Month;
```

