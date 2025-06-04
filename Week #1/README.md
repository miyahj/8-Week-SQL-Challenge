<h1 align='center'>Case Study #1: Dany's Diner</h1>
<p align='center'>
  <img src='https://github.com/user-attachments/assets/6ef64f06-0892-4339-804d-b07564e154f2' height='450'></img>
</p>

<br><h2 align='center'>Introduction</h2>
<p align='center'>
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.
</p>

<br><h2 align='center'>Problem Statement</h2>
<p align='center'>
  Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.
</p>

<br><h2 align='center'>Example Datasets and Entity Diagram</h2>
<p align='center'>
  Danny gave 3 datasets to use - sales, menu, and members - that all exist in the database, dannys_dinner.</p>

<br><h3 align='center'>Entity Diagram</h3>
<p align='center'>
  <img src=https://github.com/user-attachments/assets/71d71571-f157-48b4-8fed-d75c240f0781></img>
</p>

<br><h3 align='center'>Table 1: sales</h3>
<p align='center'>
  Captures all customer_id level purchases with a corresponding order_date and product_id information for when and what menu items were ordered.
</p>
  <table align='center'>
    <tr>
      <th>customer_id</th>
      <th>order_date</th>
      <th>product_id</th>
    </tr>
    <tr>
      <td>A</td>
      <td>2021-01-01</td>
      <td>1</td>
    </tr>
    <tr>
      <td>A</td>
      <td>2021-01-01</td>
      <td>2</td>
    </tr>
    <tr>
      <td>A</td>
      <td>2021-01-07</td>
      <td>2</td>
    </tr>
    <tr>
      <td>A</td>
      <td>2021-01-10</td>
      <td>3</td>
    </tr>
    <tr>
      <td>A</td>
      <td>2021-01-11</td>
      <td>3</td>
    </tr>
    <tr>
      <td>A</td>
      <td>2021-01-11</td>
      <td>3</td>
    </tr>
    <tr>
      <td>B</td>
      <td>2021-01-01</td>
      <td>2</td>
    </tr>
    <tr>
      <td>B</td>
      <td>2021-01-02</td>
      <td>2</td>
    </tr>
    <tr>
      <td>B</td>
      <td>2021-01-04</td>
      <td>1</td>
    </tr>
    <tr>
      <td>B</td>
      <td>2021-01-11</td>
      <td>1</td>
    </tr>
    <tr>
      <td>B</td>
      <td>2021-01-16</td>
      <td>3</td>
    </tr>
    <tr>
      <td>B</td>
      <td>2021-02-01</td>
      <td>3</td>
    </tr>
    <tr>
      <td>C</td>
      <td>2021-01-01</td>
      <td>3</td>
    </tr>
    <tr>
      <td>C</td>
      <td>2021-01-01</td>
      <td>3</td>
    </tr>
    <tr>
      <td>C</td>
      <td>2021-01-07</td>
      <td>3</td>
    </tr>
  </table>

<br><h3 align='center'>Table 2: menu</h3>
<p>
  Maps the product_id to the actual product_name and price of each menu item.
</p>
  <table align='center'>
    <tr>
      <th>product_id</th>
      <th>product_name</th>
      <th>price</th>
    </tr>
    <tr>
      <td>1</td>
      <td>sushi</td>
      <td>10</td>
    </tr>
    <tr>
      <td>2</td>
      <td>curry</td>
      <td>15</td>
    </tr>
    <tr>
      <td>3</td>
      <td>ramen</td>
      <td>12</td>
    </tr>
  </table>

<br><h3 align='center'>Table 3: members</h3>
<p align='center'>
  Captures the join_date when a customer_id joined the beta version of the Danny's Diner loyalty program.
</p>
  <table align='center'>
    <tr>
      <th>customer_id</th>
      <th>join_date</th>
    </tr>
    <tr>
      <td>A</td>
      <td>2021-01-07</td>
    </tr>
    <tr>
      <td>B</td>
      <td>2021-01-09</td>
    </tr>
  </table>

<br><h2 align='center'>Case Study Questions</h2>
<p align='center'> 
  There are 10 questions for the case study along with two bonus questions.
</p>

1. What is the total amount each customer spent at the restaurant?
```sql
```

2. How many days has each customer visited the restaurant?
  ```sql
  ```
  
3. What was the first item from the menu purchased by each customer?
  ```sql
  ```
  
4. What is the most purchased item on the menu and how many times was it purchased by all   customers?
  ```sql
  ```
  
5. Which item was the most popular for each customer?
  ```sql
  ```
  
6. Which item was purchased first by the customer after they became a member?
  ```sql
  ```
  
7. Which item was purchased just before the customer became a member?
  ```sql
  ```
  
8. What is the total items and amount spent for each member before they became a member?
  ```sql
  ```

9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
  ```sql
  ```
  
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
  ```sql
  ```



