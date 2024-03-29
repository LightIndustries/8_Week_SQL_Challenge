# Case Study #1: Danny's Diner

## Business Problem
Danny's restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business. Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.
The questions are:
1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

## The Data
There are three datasets:
* sales
* menu
* members

The datasets are related as follows

![image](https://github.com/LightIndustries/8_Week_SQL_Challenge/assets/52246820/324d2f38-3235-4cff-a1cd-3cc4bc78546d)

## Solutions to the Questions
### 1. What is the total amount each customer spent at the restaurant?
#### QUERY
```SQL
SELECT
  	customer_id,
    SUM(price) as amount_spent
FROM dannys_diner.sales
LEFT JOIN dannys_diner.menu ON dannys_diner.sales.product_id=dannys_diner.menu.product_id
GROUP BY customer_id
ORDER BY customer_id ASC;
```
#### ANSWER
|customer_id |amount_spent |
|------------|-------------|
|A           |        	 76|
|B           |           74|
|C	         |           36|

### 2. How many days has each customer visited the restaurant?
#### QUERY
```SQL
SELECT
  	customer_id,
    COUNT(DISTINCT order_date) as num_days_visited
FROM dannys_diner.sales
GROUP BY customer_id;
```
#### ANSWER
|customer_id |days_visited |
|------------|-------------|
|A           |        	 4|
|B           |           6|
|C	         |           2|

### 3. What was the first item from the menu purchased by each customer?
#### QUERY
```SQL
SELECT DISTINCT customer_id, product_name FROM
(SELECT
  	customer_id,
    order_date,
    product_name,
    RANK() OVER (PARTITION BY customer_id ORDER BY order_date ASC) as rank
FROM dannys_diner.sales
LEFT JOIN dannys_diner.menu ON sales.product_id=menu.product_id) win_query
WHERE win_query.rank = 1;
```
#### ANSWER
|customer_id |product_name    |
|------------|----------------|
|A           |        	 curry|
|A           |           sushi|
|B	         |           curry|
|C	         |           ramen|
