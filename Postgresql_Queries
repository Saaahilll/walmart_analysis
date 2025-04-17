--**Walmart Project Queries**

```sql
select * from walmart
```
```sql
select count(*) from walmart
```
```sql
select 	
	payment_method,
	count(*)
from walmart
group by payment_method;
```
```sql
select max(quantity) from walmart;
```



 1.**Find different payment method and number of transactions, number of quantity sold.**
```sql
SELECT
    payment_method,
    COUNT(*) AS number_of_transactions,
    SUM(quantity) AS total_quantity_sold
FROM
    walmart
GROUP BY
    payment_method;
```




2. **Identify the highest-rated category in each branch, displaying the branch, category and average rating**

```sql
SELECT branch, category, avg_rating
FROM (
    SELECT
        branch,
        category,
        AVG(rating) AS avg_rating,
        ROW_NUMBER() OVER (PARTITION BY branch ORDER BY AVG(rating) DESC) AS rn
    FROM walmart
    GROUP BY branch, category
) sub
WHERE rn = 1;
```



3. **Identify the busiest day for each branch based on the number of transactions.**
```sql
SELECT branch, date, transaction_count
FROM (
    SELECT
        branch,
        date,
        COUNT(*) AS transaction_count,
        ROW_NUMBER() OVER (PARTITION BY branch ORDER BY COUNT(*) DESC) AS rn
    FROM walmart
    GROUP BY branch, date
) sub
WHERE rn = 1;
```
**Finds the specific date with the highest transactions for each branch**
**Shows the exact date with the most transactions, not the day of the week.**

```sql
SELECT * 
FROM
	(SELECT 
		branch,
		TO_CHAR(TO_DATE(date, 'DD/MM/YY'), 'Day') as day_name,
		COUNT(*) as no_transactions,
		RANK() OVER(PARTITION BY branch ORDER BY COUNT(*) DESC) as rank
	FROM walmart
	GROUP BY 1, 2
	)
WHERE rank = 1
```
**Converts the date column to a day of the week**
**Shows which day of the week has the most transactions.**




4. **Calculate the total quantity of items sold per payment method. List payment_method and total_quantity.**

```sql
SELECT 
    payment_method,
    SUM(quantity) AS total_quantity
FROM 
    walmart
GROUP BY 
    payment_method;
```


5. **Determine the average, minimum, and maximum rating of category for each city. List the city, average_rating, min_rating, and max_rating.**

```sql
SELECT 
    city, 
	category,
    AVG(rating) AS average_rating,
    MIN(rating) AS min_rating,
    MAX(rating) AS max_rating
FROM 
    walmart
GROUP BY 
    city, category;
```



6. **Calculate the total profit for each category by considering total_profit as(unit_price * quantity * profit_margin).List category and total_profit, ordered from highest to lowest profit.**

```sql
SELECT
    category,
    SUM(unit_price * quantity * profit_margin) AS total_profit
FROM
    walmart
GROUP BY
    category
ORDER BY
    total_profit DESC;
```
--or can also be done by:
```sql
SELECT 
	category,
	SUM(total_amount) as total_revenue,
	SUM(total_amount * profit_margin) as profit
FROM walmart
GROUP BY 1
```


7. **Determine the most common payment method for each Branch. Display Branch and the preferred_payment_method.**

```sql
SELECT branch, payment_method AS preferred_payment_method
FROM (
    SELECT
        branch,
        payment_method,
        COUNT(*) AS method_count,
        ROW_NUMBER() OVER (PARTITION BY branch ORDER BY COUNT(*) DESC) AS rn
    FROM walmart
    GROUP BY branch, payment_method
) ranked
WHERE rn = 1;
```
--Uses ROW_NUMBER() window function.
--Returns only one payment method per branch, even if there are ties
--Returns a single preferred payment method per branch.

```sql
WITH cte 
AS
(SELECT 
	branch,
	payment_method,
	COUNT(*) as total_trans,
	RANK() OVER(PARTITION BY branch ORDER BY COUNT(*) DESC) as rank
FROM walmart
GROUP BY 1, 2
)
SELECT *
FROM cte
WHERE rank = 1
```
--Uses RANK() window function.
--If there are ties for the top payment method count within a branch, all tied methods will be included.
--Returns all payment methods tied for most frequent per branch.





8. **Categorize sales into 3 group MORNING, AFTERNOON, EVENING. Find out each of the shift and number of invoices**

```sql
SELECT
    CASE
        WHEN EXTRACT(HOUR FROM time) BETWEEN 6 AND 11 THEN 'MORNING'
        WHEN EXTRACT(HOUR FROM time) BETWEEN 12 AND 17 THEN 'AFTERNOON'
        WHEN EXTRACT(HOUR FROM time) BETWEEN 18 AND 23 THEN 'EVENING'
        ELSE 'NIGHT'
    END AS shift,
    COUNT(*) AS number_of_invoices
FROM walmart
GROUP BY shift
ORDER BY shift;
```
--The error occurs because your time column is of type text (string), but the EXTRACT function in PostgreSQL requires a value of type TIME, DATE, or TIMESTAMP as its source

```sql
SELECT
    CASE
        WHEN EXTRACT(HOUR FROM time::time) BETWEEN 6 AND 11 THEN 'MORNING'
        WHEN EXTRACT(HOUR FROM time::time) BETWEEN 12 AND 17 THEN 'AFTERNOON'
        WHEN EXTRACT(HOUR FROM time::time) BETWEEN 18 AND 23 THEN 'EVENING'
        ELSE 'NIGHT'
    END AS shift,
    COUNT(*) AS number_of_invoices
FROM walmart
GROUP BY shift
ORDER BY shift;
```
--time::time casts the time column from text to PostgreSQL’s TIME type, allowing EXTRACT(HOUR FROM ...) to work




9. **Identify 5 branch with highest decrese ratio in revevenue compare to last year(current year 2023 and last year 2022).**

```sql
WITH yearly_revenue AS (
    SELECT
        branch,
        EXTRACT(YEAR FROM date) AS year,
        SUM(total_amount) AS revenue
    FROM walmart
    WHERE EXTRACT(YEAR FROM date) IN (2022, 2023)
    GROUP BY branch, year
),
revenue_comparison AS (
    SELECT
        branch,
        year,
        revenue,
        LAG(revenue) OVER (PARTITION BY branch ORDER BY year) AS prev_year_revenue
    FROM yearly_revenue
)
SELECT
    branch,
    ROUND(((revenue - prev_year_revenue) / prev_year_revenue) * 100, 2) AS revenue_decrease_ratio
FROM revenue_comparison
WHERE year = 2023
  AND prev_year_revenue IS NOT NULL
  AND revenue < prev_year_revenue
ORDER BY revenue_decrease_ratio DESC
LIMIT 5;
```


```sql
WITH yearly_revenue AS (
    SELECT
        branch,
        EXTRACT(YEAR FROM date::date) AS year,
        SUM(total_amount) AS revenue
    FROM walmart
    WHERE EXTRACT(YEAR FROM date::date) IN (2022, 2023)
    GROUP BY branch, year
),
revenue_comparison AS (
    SELECT
        branch,
        year,
        revenue,
        LAG(revenue) OVER (PARTITION BY branch ORDER BY year) AS prev_year_revenue
    FROM yearly_revenue
)
SELECT
    branch,
    ROUND(((revenue - prev_year_revenue) / prev_year_revenue) * 100, 2) AS revenue_decrease_ratio
FROM revenue_comparison
WHERE year = 2023
  AND prev_year_revenue IS NOT NULL
  AND revenue < prev_year_revenue
ORDER BY revenue_decrease_ratio DESC
LIMIT 5;
```


```sql
WITH yearly_revenue AS (
    SELECT
        branch,
        EXTRACT(YEAR FROM date::date) AS year,
        SUM(total_amount) AS revenue
    FROM walmart
    WHERE EXTRACT(YEAR FROM date::date) IN (2022, 2023)
    GROUP BY branch, year
),
revenue_comparison AS (
    SELECT
        branch,
        year,
        revenue,
        LAG(revenue) OVER (PARTITION BY branch ORDER BY year) AS prev_year_revenue
    FROM yearly_revenue
)
SELECT
    branch,
    ROUND(
      ((revenue - prev_year_revenue) / prev_year_revenue)::numeric,
      2
    ) * 100 AS revenue_decrease_ratio
FROM revenue_comparison
WHERE year = 2023
  AND prev_year_revenue IS NOT NULL
  AND revenue < prev_year_revenue
ORDER BY revenue_decrease_ratio DESC
LIMIT 5;
```
**First three queries couldn't run because date column is stored as a text type, not as a DATE type. The EXTRACT(YEAR FROM date) function only works on columns of type DATE, TIMESTAMP, or similar.**



** taking help from the perplexity-ai to fix the error in the query:**
```sql
WITH revenue_2022 AS (
    SELECT 
        branch,
        SUM(total_amount) AS revenue
    FROM walmart
    WHERE EXTRACT(YEAR FROM TO_DATE(date, 'DD/MM/YY')) = 2022
    GROUP BY branch
),
revenue_2023 AS (
    SELECT 
        branch,
        SUM(total_amount) AS revenue
    FROM walmart
    WHERE EXTRACT(YEAR FROM TO_DATE(date, 'DD/MM/YY')) = 2023
    GROUP BY branch
)
SELECT 
    ls.branch,
    ls.revenue AS last_year_revenue,
    cs.revenue AS cr_year_revenue,
    ROUND(
        ((ls.revenue - cs.revenue)::numeric / ls.revenue::numeric) * 100, 
        2
    ) AS rev_dec_ratio
FROM revenue_2022 AS ls
JOIN revenue_2023 AS cs ON ls.branch = cs.branch
WHERE ls.revenue > cs.revenue
ORDER BY rev_dec_ratio DESC
LIMIT 5;
```
