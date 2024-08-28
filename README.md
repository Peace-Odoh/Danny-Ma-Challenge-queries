# Danny-Ma-Challenge-queries
--Question 1 Query #2

select  a.customer_id, sum(b.price) from dannys_diner.sales a
join dannys_diner.menu b on a.product_id = b.product_id
Group by a.customer_id
order by a.customer_id;

--Question 2 Query #3
select customer_id, count(order_date)
from dannys_diner.sales
group by customer_id
order by customer_id;

--Question 3 Query #4
Select a.customer_id, a.order_date, b.product_name
from dannys_diner.sales a join dannys_diner.menu b 
on a.product_id = b.product_id 
order by order_date ASC
LIMIT 3 offset 1;

--Question 4 Query #5
select a.customer_id, b.product_name,count(a.product_id) as count
from dannys_diner.sales a join dannys_diner.menu b
on a.product_id = b.product_id
group by customer_id,
         product_name
order by count desc
LIMIT 3;

----Question 5 Query #6
select a.customer_id, b.product_name,count(a.product_id) as count
from dannys_diner.sales a join dannys_diner.menu b
on a.product_id = b.product_id
group by customer_id,
         product_name
order by count desc
LIMIT 3;


--Question 6 Query #7
WITH FirstPurchase AS (
    SELECT 
        a.customer_id, 
        c.product_name, 
        b.order_date, 
        a.join_date,
        ROW_NUMBER() OVER(PARTITION BY a.customer_id ORDER BY b.order_date ASC) AS rn
    FROM 
        dannys_diner.members a
    JOIN 
        dannys_diner.sales b 
    ON 
        a.customer_id = b.customer_id
    JOIN 
        dannys_diner.menu c 
    ON 
        b.product_id = c.product_id
    WHERE 
        b.order_date >= a.join_date
        AND a.join_date BETWEEN '2021-01-07' AND '2021-01-09'
)

SELECT 
    customer_id, 
    product_name, 
    join_date
FROM 
    FirstPurchase
WHERE 
    rn = 1
ORDER BY 
    customer_id ASC;

Question 7 Query #8
WITH FirstPurchase AS (
    SELECT 
        a.customer_id, 
        c.product_name, 
        b.order_date, 
        a.join_date,
        ROW_NUMBER() OVER(PARTITION BY a.customer_id ORDER BY b.order_date ASC) AS rn
    FROM 
        dannys_diner.members a
    JOIN 
        dannys_diner.sales b 
    ON 
        a.customer_id = b.customer_id
    JOIN 
        dannys_diner.menu c 
    ON 
        b.product_id = c.product_id
    WHERE 
        b.order_date <= a.join_date
        

SELECT 
    customer_id, 
    product_name, 
    join_date
FROM 
    FirstPurchase
WHERE 
    rn = 1
ORDER BY 
    customer_id ASC;

--Question 8 Query 9
SELECT 
     a.customer_id,
     COUNT(a.product_id) AS Total_Items_Bought,
     SUM(b.price) AS total_spent
FROM 
     dannys_diner.sales a
JOIN 
     dannys_diner.menu b
ON 
     a.product_id = b.product_id
JOIN 
     dannys_diner.members c
ON 
     a.customer_id = c.customer_id
WHERE 
     a.order_date > c.join_date
GROUP BY
     a.customer_id;

--Question 9 Query #10
SELECT 
    a.customer_id, 
    SUM(
        CASE 
            WHEN b.product_id = '1' THEN b.price * 20
            WHEN b.product_id = '2' THEN b.price * 10
            WHEN b.product_id = '3' THEN b.price * 10
            ELSE 0
        END
    ) AS POINTS
FROM 
    dannys_diner.sales a 
JOIN 
    dannys_diner.menu b
ON 
    a.product_id = b.product_id
GROUP BY 
    a.customer_id;

--Question 10 Query #11
SELECT 
    a.customer_id, 
    SUM(
        CASE 
            WHEN b.product_id = '1' THEN b.price * 20
            WHEN b.product_id = '2' THEN b.price * 20
            WHEN b.product_id = '3' THEN b.price * 20
            ELSE 0
        END
    ) AS POINTS
FROM 
    dannys_diner.sales a 
JOIN 
    dannys_diner.menu b
ON 
    a.product_id = b.product_id
JOIN dannys_diner.members c
ON a.customer_id = c.customer_id 
WHERE join_date >= order_date
GROUP BY 
    a.customer_id;



