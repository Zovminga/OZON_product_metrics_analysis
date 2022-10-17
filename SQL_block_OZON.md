## I. SQL block

First we write some SQL queries to work with Ozon databases in Clickhouse

### 1. Calculate the % change in the number of customers who made a purchase, month-to-month  
**Columns:**   
client_id - *integer*  
order_id - *integer*  
order_date - *date*  

# SQL query:
SELECT month_date,
((clients_number - clients_prev) * 100 / clients_prev) AS clients_diff,
FROM (
    SELECT COUNT(client_id) AS clients_number, DATE_TRUNC('month', order_date)::date AS month_date, LAG(COUNT(client_id), 1, 0) OVER (ORDER BY month_date) AS clients_prev,
    FROM orders
    GROUP BY month_date
    ) AS orders
ORDER BY month_date

### 2. Calculate the sum of GMV (Gross Merchandise Value) with a cumulative total by day 
**Columns:**   
fact_date - *date*  
gmv - *integer*

# SQL query:
SELECT fact_date, SUM(gmv) OVER (PARTITION BY fact_date ORDER BY fact_date) AS GMV_total
FROM database 
ORDER BY fact_date

### 3. Get the response time for each letter sent by the user mr_employee@ozon.ru 
**Columns:**   
mail_id - *integer*  
mail_from - *varchar(30)*  
mail_to - *varchar(30)*  
mail_subject - *varchar(30)*  
timestamp - *timestamp*

# SQL query:
SELECT b.mail_id, (b.reply - a.reply) AS response_time, a.mail_subject
FROM mailbox AS a
JOIN mailbox AS b
ON a.mail_from = b.mail_to
WHERE a.mail_from = 'mr_employee@ozon.ru' AND a.mail_subject = b.mail_subject
ORDER BY a.mail_id

### 4. Select the id of employees with a difference in salary within 5000 rubles 
**Columns:**   
employee_id - *integer*  
salary_rub - *integer*  

# SQL query:
SELECT DISTINCT a.employee_id
  FROM database AS a, database AS b
 WHERE b.salary_rub BETWEEN (a.salary_rub - 5000)
                         AND (a.salary_rub + 5000)
   AND a.employee_id != b.employee_id