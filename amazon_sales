CREATE TABLE sales (
    invoice_id VARCHAR(30) PRIMARY KEY,
    branch VARCHAR(5),
    city VARCHAR(30),
    customer_type VARCHAR(30),
    gender VARCHAR(10),
    product_line VARCHAR(100),
    unit_price DECIMAL(10,2),
    quantity INT,
    VAT DECIMAL(6,4),
    total DECIMAL(10,2),
    date DATE,
    time TIME,
    payment_method VARCHAR(30),
    cogs DECIMAL(10,2),
    gross_margin_percentage DECIMAL(11,9),
    gross_income DECIMAL(10,2),
    rating DECIMAL(2,1)
);



ALTER TABLE sales
ADD COLUMN time_of_day VARCHAR(15);

UPDATE sales
SET time_of_day =
CASE
    WHEN HOUR(time) < 12 THEN 'Morning'
    WHEN HOUR(time) < 18 THEN 'Afternoon'
    ELSE 'Evening'
END;


ALTER TABLE sales
ADD COLUMN day_name VARCHAR(10);

UPDATE sales
SET day_name = DAYNAME(date);

ALTER TABLE sales
ADD COLUMN clean_date DATE;


UPDATE sales
SET clean_date = STR_TO_DATE(date, '%d-%m-%Y');

SELECT date, clean_date
FROM sales
LIMIT 10;

UPDATE sales
SET day_name = DAYNAME(clean_date);
SELECT DISTINCT day_name FROM sales;


ALTER TABLE sales
ADD COLUMN month_name VARCHAR(10);
UPDATE sales
SET month_name = MONTHNAME(clean_date);



-- Questions

-- Q1: Count of distinct cities
SELECT COUNT(DISTINCT city) AS city_count
FROM sales;

-- 3



-- Q2: Branch-wise city mapping
SELECT DISTINCT branch, city
FROM sales;

/*
A	Yangon
C	Naypyitaw
B	Mandalay
*/

-- Q3: Count of distinct product lines
SELECT COUNT(DISTINCT product_line) AS product_line_count
FROM sales;

-- 6

-- Q4: Most frequently used payment method
SELECT Payment, COUNT(*) AS usage_count
FROM sales
GROUP BY Payment
ORDER BY usage_count DESC
LIMIT 1;

-- 345


-- Q5: Product line with highest sales revenue
SELECT product_line, SUM(total) AS total_sales
FROM sales
GROUP BY product_line
ORDER BY total_sales DESC
LIMIT 1;

-- -- Q5: Product line with highest sales revenue
SELECT product_line, SUM(total) AS total_sales
FROM sales
GROUP BY product_line
ORDER BY total_sales DESC
LIMIT 1;

-- Food and beverages	56144.844000000005


-- Q6: Monthly revenue
SELECT month_name, SUM(total) AS monthly_revenue
FROM sales
GROUP BY month_name
ORDER BY monthly_revenue DESC;

/*
January	116291.86800000005
March	109455.50700000004
February	97219.37399999997
*/


-- Q7: Month with highest COGS
SELECT month_name, SUM(cogs) AS total_cogs
FROM sales
GROUP BY month_name
ORDER BY total_cogs DESC
LIMIT 1;

-- January	110754.16000000002


-- Q8: Product line with highest revenue
SELECT product_line, SUM(total) AS revenue
FROM sales
GROUP BY product_line
ORDER BY revenue DESC
LIMIT 1;

-- Food and beverages	56144.844000000005


-- Q9: City with highest revenue
SELECT city, SUM(total) AS revenue
FROM sales
GROUP BY city
ORDER BY revenue DESC
LIMIT 1;

-- Naypyitaw	110568.70649999994


-- Q10: Product line with highest VAT
SELECT product_line, SUM(VAT) AS total_vat
FROM sales
GROUP BY product_line
ORDER BY total_vat DESC
LIMIT 1;

-- Food and beverages	2673.5639999999994


-- Q11: Product line performance classification
SELECT product_line,
       SUM(total) AS total_sales,
       CASE
           WHEN SUM(total) > (SELECT AVG(total) FROM sales)
           THEN 'Good'
           ELSE 'Bad'
       END AS performance
FROM sales
GROUP BY product_line;

/*
Health and beauty	49193.739000000016	Good
Electronic accessories	54337.531500000005	Good
Home and lifestyle	53861.91300000001	Good
Sports and travel	55122.826499999996	Good
Food and beverages	56144.844000000005	Good
Fashion accessories	54305.895	Good
*/


-- Q12: Branches with above-average quantity sold
SELECT branch, SUM(quantity) AS total_quantity
FROM sales
GROUP BY branch
HAVING SUM(quantity) > (
    SELECT AVG(quantity) FROM sales
);

/*
A	1859
C	1831
B	1820
*/


-- Q13: Most frequent product line by gender
SELECT gender, product_line, COUNT(*) AS frequency
FROM sales
GROUP BY gender, product_line
ORDER BY gender, frequency DESC;

/*
Female	Fashion accessories	96
Female	Food and beverages	90
Female	Sports and travel	88
Female	Electronic accessories	84
Female	Home and lifestyle	79
Female	Health and beauty	64
Male	Health and beauty	88
Male	Electronic accessories	86
Male	Food and beverages	84
Male	Fashion accessories	82
Male	Home and lifestyle	81
Male	Sports and travel	78
*/



-- Q14: Average rating per product line
SELECT product_line, ROUND(AVG(rating), 2) AS avg_rating
FROM sales
GROUP BY product_line
ORDER BY avg_rating DESC;

/*
Electronic accessories	6.92
Fashion accessories	7.03
Food and beverages	7.11
Health and beauty	7
Home and lifestyle	6.84
Sports and travel	6.92
*/


-- Q15: Sales count by time of day and weekday
SELECT day_name, time_of_day, COUNT(*) AS sales_count
FROM sales
GROUP BY day_name, time_of_day
ORDER BY day_name, sales_count DESC;

/*
Friday	Afternoon	74
Friday	Evening	36
Friday	Morning	29
Monday	Afternoon	75
Monday	Evening	29
Monday	Morning	21
Saturday	Afternoon	81
Saturday	Evening	55
Saturday	Morning	28
*/


-- Q16: Customer type with highest revenue
SELECT customer_type, SUM(total) AS revenue
FROM sales
GROUP BY customer_type
ORDER BY revenue DESC
LIMIT 1;

-- Member	164223.44400000002



-- Q17: City with highest average VAT
SELECT city, AVG(VAT) AS avg_vat
FROM sales
GROUP BY city
ORDER BY avg_vat DESC
LIMIT 1;

-- Naypyitaw	16.05236737804879


-- Q18: Customer type paying highest VAT
SELECT customer_type, SUM(VAT) AS total_vat
FROM sales
GROUP BY customer_type
ORDER BY total_vat DESC
LIMIT 1;

-- Member	7820.164000000002


-- Q19: Count of distinct customer types
SELECT COUNT(DISTINCT customer_type) AS customer_type_count
FROM sales;

-- 2



-- Q20: Count of distinct payment methods
SELECT COUNT(DISTINCT Payment) AS payment_method_count
FROM sales;

-- 3


-- Q21: Most frequent customer type
SELECT customer_type, COUNT(*) AS frequency
FROM sales
GROUP BY customer_type
ORDER BY frequency DESC
LIMIT 1;

-- Member	501



-- Q22: Customer type with highest purchase frequency
SELECT customer_type, COUNT(*) AS purchase_count
FROM sales
GROUP BY customer_type
ORDER BY purchase_count DESC
LIMIT 1;

-- Member	501



-- Q23: Predominant customer gender
SELECT gender, COUNT(*) AS count
FROM sales
GROUP BY gender
ORDER BY count DESC
LIMIT 1;

-- Female	501



-- Q24: Gender distribution by branch
SELECT branch, gender, COUNT(*) AS count
FROM sales
GROUP BY branch, gender
ORDER BY branch, count DESC;

/*
A	Male	179
A	Female	161
B	Male	170
B	Female	162
C	Female	178
C	Male	150
*/



-- Q25: Time of day with most ratings
SELECT time_of_day, COUNT(rating) AS rating_count
FROM sales
GROUP BY time_of_day
ORDER BY rating_count DESC
LIMIT 1;

-- Afternoon	528

