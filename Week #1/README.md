<h1 align='center'>Case Study #1: Dany's Diner</h1>
<p align='center'>
  <img src='https://github.com/user-attachments/assets/6ef64f06-0892-4339-804d-b07564e154f2' height='450'></img>
</p>

<br><h2 align='center'>Introduction & Problem Statement</h2>
<p align='center'>
Danny opened a Japanese restaruant in 2021 that sells his favorite food: sushi, curry, and ramen. Dany is looking for ways to keep his business afloat by analyzing customer visiting patterns, how much money they [the customers] have spent, their favorite food items, etc. He will use the insights to improve the existing customer loyalty program. Dany also wants to create datatsets for his team to reference without having to use SQL.
</p>

<br><h2 align='center'>Entity Diagram</h2>
<p align='center'>
  Danny gave 3 datasets to use - sales, menu, and members - that all exist in one database.</p>
  
<p align='center'>
  <img src=https://github.com/user-attachments/assets/71d71571-f157-48b4-8fed-d75c240f0781></img>
</p>

<br><h2 align='center'>Case Study Questions</h2>
<p align='center'> 
  There are 10 questions for the case study along with two bonus questions.
</p>

1. What is the total amount each customer spent at the restaurant?
```sql
SELECT
  sa.customer_id,
  SUM(me.price) AS total_spent
FROM sales AS sa
LEFT JOIN menu AS me
  ON sa.product_id = me.product_id
GROUP BY sa.customer_id
ORDER BY sa.customer_id;
```
<table align='center'>
  <tr>
    <th>customer_id</th>
    <th>total_spent</th>
  </tr>
  <tr>
    <td>A</td>
    <td>76</td>
  </tr>
  <tr>
    <td>B</td>
    <td>74</td>
  </tr>
  <tr>
    <td>C</td>
    <td>36</td>
  </tr>
</table>

2. How many days has each customer visited the restaurant?
  ```sql
SELECT
    customer_id,
    COUNT(DISTINCT order_date) AS days_visited
FROM sales
GROUP BY customer_id
ORDER BY customer_id;
  ```
<table align='center'>
  <tr>
    <th>customer_id</th>
    <th>days_visited</th>
  </tr>
  <tr>
    <td>A</td>
    <td>4</td>
  </tr>
  <tr>
    <td>B</td>
    <td>6</td>
  </tr>
  <tr>
    <td>C</td>
    <td>2</td>
  </tr>
</table>

3. What was the first item from the menu purchased by each customer?
  ```sql
WITH ordered_dates AS (
SELECT
    sa.customer_id, 
    sa.order_date, 
    me.product_name, 
    DENSE_RANK() OVER(
      PARTITION BY sa.customer_id
      ORDER BY sa.order_date ASC) AS rank
FROM sales AS sa
INNER JOIN menu AS me
	ON sa.product_id = me.product_id
)
SELECT
    customer_id,
    product_name
FROM ordered_dates
WHERE rank = 1
GROUP BY
  customer_id,
  product_name;
  ```
<table align='center'>
  <tr>
    <th>customer_id</th>
    <th>product_name</th>
  </tr>
  <tr>
    <td>A</td>
    <td>curry</td>
  </tr>
  <tr>
    <td>A</td>
    <td>sushi</td>
  </tr>
  <tr>
    <td>B</td>
    <td>curry</td>
  </tr>
  <tr>
    <td>C</td>
    <td>ramen</td>
  </tr>
</table>

4. What is the most purchased item on the menu and how many times was it purchased by all   customers?
  ```sql
SELECT
	me.product_name,
    COUNT(sa.product_id) AS most_purchased
FROM menu AS me
INNER JOIN sales AS sa
	ON me.product_id = sa.product_id
GROUP BY me.product_name
ORDER BY most_purchased DESC
LIMIT 1;
  ```
  <table align='center'>
    <tr>
    <th>product_name</th>
    <th>most_purchased</th>
  </tr>
    <tr>
    <td>ramen</td>
    <td>8</td>
  </tr>
  </table>
  
5. Which item was the most popular for each customer?
  ```sql
WITH popular_items AS (
SELECT
	sa.customer_id,
    me.product_name,
    COUNT(*) AS order_count,
    DENSE_RANK() OVER(
      PARTITION BY sa.customer_id
      ORDER BY COUNT(*) DESC) AS rank
FROM sales AS sa
INNER JOIN menu AS me
	ON sa.product_id = me.product_id
GROUP BY sa.customer_id, me.product_name
  )
SELECT
	customer_id,
    product_name,
    order_count
FROM popular_items
WHERE rank = 1
GROUP BY 
	customer_id, 
    product_name,
    order_count;
  ```
<table align='center'>
    <tr>
      <th>customer_id</th>
      <th>product_name</th>
      <th>order_count</th>
    </tr>
    <tr>
      <td>A</td>
      <td>ramen</td>
      <td>3</td>
    </tr>
    <tr>
      <td>B</td>
      <td>curry</td>
      <td>2</td>
    </tr>
    <tr>
      <td>B</td>
      <td>ramen</td>
      <td>2</td>
    </tr>
    <tr>
      <td>B</td>
      <td>sushi</td>
      <td>2</td>
    </tr>
    <tr>
      <td>C</td>
      <td>ramen</td>
      <td>3</td>
    </tr>
  </table>
  
6. Which item was purchased first by the customer after they became a member?
  ```sql
WITH joined_as_member AS (
  SELECT 
  	mem.customer_id,
  	sa.product_id,
  	ROW_NUMBER() OVER(
      PARTITION BY mem.customer_id 
      ORDER BY sa.order_date) AS row_num
 FROM members AS mem
 INNER JOIN sales AS sa
  	ON mem.customer_id = sa.customer_id
  	AND sa.order_date > mem.join_date
 )
 SELECT
 	customer_id,
    product_name AS ordered_after_membership
FROM joined_as_member
INNER JOIN menu AS me
	ON joined_as_member.product_id = me.product_id
WHERE row_num = 1
ORDER BY customer_id;
  ```
<table align='center'>
  <tr>
    <th>customer_id</th>
    <th>ordered_after_membership</th>
  </tr>
  <tr>
    <td>A</td>
    <td>ramen</td>
  </tr>
  <tr>
    <td>B</td>
    <td>sushi</td>
  </tr>
  </table>
  
7. Which item was purchased just before the customer became a member?
  ```sql
WITH prior_member_purchase AS (
  SELECT 
  	mem.customer_id,
  	sa.product_id,
  	ROW_NUMBER() OVER(
      PARTITION BY mem.customer_id 
      ORDER BY sa.order_date DESC) AS row_num
 FROM members AS mem
 INNER JOIN sales AS sa
  	ON mem.customer_id = sa.customer_id
  	AND sa.order_date < mem.join_date
 )
 SELECT
 	customer_id,
    product_name AS ordered_before_membership
FROM prior_member_purchase
INNER JOIN menu AS me
	ON prior_member_purchase.product_id = me.product_id
WHERE row_num = 1
ORDER BY customer_id;
  ```
<table align='center'>
  <tr>
   <th>customer_id</th>
    <th>ordered_before_membership</th>
  </tr>
  <tr>
    <td>A</td>
    <td>sushi</td>
  </tr>
  <tr>
    <td>B</td>
    <td>sushi</td>
  </tr>
  </table>
  
8. What is the total items and amount spent for each member before they became a member?
  ```sql
 SELECT
  	sa.customer_id,
    COUNT(sa.product_id) AS total_items,
  	SUM(me.price) AS total_sales
 FROM sales AS sa
 INNER JOIN members AS mem
  ON sa.customer_id = mem.customer_id
  	AND sa.order_date < mem.join_date
 INNER JOIN menu AS me
 	ON sa.product_id = me.product_id
GROUP BY sa.customer_id
ORDER BY sa.customer_id;
  ```
<table align='center'>
  <tr>
    <th>customer_id</th>
    <th>total_items</th>
    <th>total_sales</th>
  </tr>
  <tr>
    <td>A</td>
    <td>2</td>
    <td>25</td>
  </tr>
  <tr>
    <td>B</td>
    <td>3</td>
    <td>40</td>
  </tr>
  </table>
  
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
  ```sql
WITH point_count AS (
	SELECT
	product_id,
    CASE
    	WHEN product_id = 1
        	THEN price * 20
    	ELSE price * 10
  	END AS points
FROM menu)
SELECT 
	sa.customer_id,
    SUM(point_count.points) AS total_points
FROM point_count
INNER JOIN sales as sa
	ON point_count.product_id = sa.product_id
GROUP BY 
	sa.customer_id
ORDER BY
	sa.customer_id;
  ```
<table align='center'>
  <tr>
    <th>customer_id</th>
    <th>total_points</th>
  </tr>
  <tr>
    <td>A</td>
    <td>860</td>
  </tr>
  <tr>
    <td>B</td>
    <td>940</td>
  </tr>
  <tr>
    <td>C</td>
    <td>360</td>
  </tr>
  </table>
  
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
```sql
WITH first_week_points AS(
	SELECT
	customer_id,
  	join_date,
  	join_date + 6 AS first_week,
  	DATE_TRUNC(
      'month',
      '2021-01-01'
      ::DATE
     )
  	+ interval '1 month'
  	- interval '1 day' AS last_day
	FROM members
)
SELECT
	sa.customer_id,
	SUM(
		CASE
			WHEN me.product_id = 1
				THEN me.price * 20
			WHEN sa.order_date BETWEEN fwp.join_date AND fwp.first_week
				THEN me.price * 20
			ELSE me.price * 10
		END
	) AS points
FROM sales AS sa
INNER JOIN first_week_points AS fwp
	ON sa.customer_id = fwp.customer_id
		AND fwp.join_date <= sa.order_date
    AND sa.order_date <= fwp.last_day
INNER JOIN menu AS me
	ON sa.product_id = me.product_id
GROUP BY sa.customer_id
ORDER BY sa.customer_id;
```
<table align='center'>
	<tr>
    <th>customer_id</th>
    <th>points</th>
  </tr>
  <tr>
    <td>A</td>
    <td>1020</td>
  </tr>
	<tr>
    <td>B</td>
    <td>320</td>
  </tr>
  </table>


