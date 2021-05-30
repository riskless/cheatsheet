### Prepare a list for which salesman are working for which customer along with city and commissions earned by the salesman
- customer: customer_id, cust_name, city, grade, salesman_id 
- salesman: salesman_id, name, city, commission 
```sql
SELECT a.cust_name AS "Customer Name", 
a.city, b.name AS "Salesman", b.commission 
FROM customer a 
INNER JOIN salesman b 
ON a.salesman_id=b.salesman_id;
```

- https://www.w3resource.com/sql-exercises/sql-joins-exercise-3.php
### Prepare a list with order no, purchase amount, customer name and their cities for those orders which order amount between 500 and 2000
- orders: ord_no, purch_amt, ord_date, customer_id, salesman_id
- customer: customer_id, cust_name, city, grade, salesman_id 
```sql
SELECT a.ord_no, a.purch_amt, b.cust_name, b.city 
FROM orders a,customer b 
WHERE a.customer_id = b.customer_id 
	AND a.purch_amt BETWEEN 500 AND 2000;
```

### Find the salesmen and customers with their name and cities, who belongs to the same city
- salesman: salesman_id, name, city, commission 
- customer: customer_id, cust_name, city, grade, salesman_id 
```sql
SELECT salesman.name AS "Salesman", customer.cust_name, customer.city 
FROM salesman, customer 
WHERE salesman.city = customer.city;
```
