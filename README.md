# SQL_QUERIES
--- SINGLE TABLE RETRIEVAL
--1)Find out the names of all the clients.
SELECT NAME FROM CLIENT_MASTER;

--2)Print the entire client_master table.
SELECT * FROM CLIENT_MASTER;

--3)Retrieve the list of names and the cities of all the clients
SELECT NAME,CITY FROM CLIENT_MASTER;

--4)List the various products available from the product_master table.
SELECT DESCRIPTION FROM PRODUCT_MASTER;

--5)Find the names of all clients having ‘a’ as the second letter in their table
SELECT NAME
FROM CLIENT_MASTER
WHERE NAME LIKE '_a%';

--6)Find the names of all clients who stay in a city whose second letter is ‘a’
SELECT NAME 
FROM CLIENT_MASTER 
WHERE city LIKE '_a%';

--7)Find out the clients who stay in a city ‘Bombay’ or city ‘Delhi’ or city ‘Madras’.
SELECT NAME,CLIENT_NO,CITY
FROM CLIENT_MASTER
WHERE CITY='Delhi'OR CITY='Bombay' OR CITY='Madras';

--8)List all the clients who are located in Bombay.
SELECT * FROM CLIENT_MASTER
WHERE CITY='Bombay';

--9)Print the list of clients whose bal_due are greater than value 10000
SELECT NAME,CLIENT_NO 
FROM CLIENT_MASTER
WHERE BAL_DUE>10000;

--10)Print the information from sales_order table of orders placed in the month of January.
SELECT * 
FROM SALES_ORDER
WHERE EXTRACT(MONTH FROM S_ORDER_DATE) = 01;

--11)Display the order information for client_no ‘C00001’ and ‘C00002’
SELECT * FROM SALES_ORDER
WHERE CLIENT_NO IN ('C00001','C00002');

--12)Find the products with description as ‘1.44 Drive’ and ‘1.22 Drive’
SELECT * FROM PRODUCT_MASTER 
WHERE DESCRIPTION IN ('1.44 Drive','1.22 Drive');

--13)Find the products whose selling price is greater than 2000 and less than or equal to 5000
SELECT * FROM PRODUCT_MASTER
WHERE SELL_PRICE >2000 AND SELL_PRICE<=5000;

--14)Find the products whose selling price is more than 1500 and also find the new selling price as original selling price * 15
SELECT PRODUCT_NO, DESCRIPTION,SELL_PRICE*15 
FROM PRODUCT_MASTER 
WHERE SELL_PRICE > 1500;

--15)Rename the new column in the above query as new_price
ALTER TABLE PRODUCT_MASTER RENAME SELL_PRICE TO NEW_PRICE;

--16)Find the products whose cost price is less than 1500
SELECT PRODUCT_NO, DESCRIPTION 
FROM PRODUCT_MASTER 
WHERE COST_PRICE<1500;

--17)List the products in sorted order of their description.
SELECT * FROM PRODUCT_MASTER 
ORDER BY DESCRIPTION ASC;

--18)Calculate the square root the price of each product.
SELECT SQRT(SELL_PRICE),SQRT(COST_PRICE) 
FROM PRODUCT_MASTER;

--19)Divide the cost of product ‘540 HDD’ by difference between its price and 100
UPDATE product_master 
SET cost_price = (cost_price\(cost_price-100))
WHERE description = '540 HDD';

--20)List the names, city and state of clients not in the state of Maharashtra
SELECT NAME,CITY,STATE 
FROM CLIENT_MASTER 
WHERE NOT STATE ='MAHARASHTRA';

--21)List the product_no, description, sell_price of products whose description begin with letter ‘M’
SELECT PRODUCT_NO,DESCRIPTION ,SELL_PRICE 
FROM PRODUCT_MASTER
WHERE DESCRIPTION LIKE 'M%';

--22)List all the orders that were canceled in the month of May.
SELECT S_ORDER_NO,ORDER_STATUS 
FROM SALES_ORDER 
WHERE EXTRACT(MONTH FROM S_ORDER_DATE) = 05 
AND ORDER_STATUS= 'CANCELED';

-- SET FUNCTIONS AND CONCATENATION 
--23)Count the total number of orders.
SELECT COUNT(S_ORDER_NO) FROM SALES_ORDER;

--24)Calculate the average price of all the products.
SELECT AVG(SELL_PRICE), AVG(COST_PRICE)
FROM PRODUCT_MASTER;

--25)Calculate the minimum price of products.
SELECT MIN(SELL_PRICE), MIN(COST_PRICE) 
FROM PRODUCT_MASTER;

--26)Determine the maximum and minimum product prices. Rename the title as max_price and min_price respectively.
SELECT MAX(SELL_PRICE),MIN(SELL_PRICE),MAX(COST_PRICE),MIN(COST_PRICE)
FROM PRODUCT_MASTER;

--27)Count the number of products having price greater than or equal to 1500.
SELECT PRODUCT_NO,DESCRIPTION,SELL_PRICE 
FROM PRODUCT_MASTER WHERE SELL_PRICE>=1500;

--28) Find all the products whose qty_on_hand is less than reorder level.
ALTER TABLE PRODUCT_MASTER ADD QTY_ON_HAND NUMBER(10);
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=100 WHERE PRODUCT_NO='P00001';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=10 WHERE PRODUCT_NO='P03453';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=20 WHERE PRODUCT_NO='P06734';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=100 WHERE PRODUCT_NO='P07865';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=10 WHERE PRODUCT_NO='P07868';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=10 WHERE PRODUCT_NO='P07885';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=10 WHERE PRODUCT_NO='P07965';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=10 WHERE PRODUCT_NO='P07975';
UPDATE PRODUCT_MASTER SET QTY_ON_HAND=2 WHERE PRODUCT_NO='P08865';

SELECT PRODUCT_NO,QTY_ON_HAND,RECORD_LVL
FROM PRODUCT_MASTER
WHERE QTY_ON_HAND<RECORD_LVL;

--29)Print the information of client_master, product_master, sales_order table in the following formate for all the records
--{cust_name} has placed order {order_no} on {s_order_date}.
SELECT name || 'has placed order'|| s_order_no || 'on' || s_order_date
from Client_master c, sales_order s
WHERE s.client_no = c.client_no;

--HAVING AND GROUP BY
--30)Print the description and total qty sold for each product.
SELECT P.PRODUCT_NO, P.DESCRIPTION, SUM(S.QTY_DISP) AS TOTAL_QTY_SOLD
FROM PRODUCT_MASTER P
LEFT JOIN SALES_ORDER_DETAILS S ON P.PRODUCT_NO =S.PRODUCT_NO
GROUP BY P.PRODUCT_NO, P.DESCRIPTION;

--31)Find the value of each product sold.
SELECT S.PRODUCT_NO, P.DESCRIPTION, SUM(S.QTY_DISP) AS TOTAL_QTY_SOLD,
SUM(S.QTY_DISP*P.SELL_PRICE) AS TOTAL_VALUE_SOLD
FROM SALES_ORDER_DETAILS S
INNER JOIN PRODUCT_MASTER P ON S.PRODUCT_NO =P.PRODUCT_NO
GROUP BY S.PRODUCT_NO, P.DESCRIPTION;

--32)Calculate the average qty sold for each client that has a maximum order value of 15000.00
SELECT S.CLIENT_NO,AVG(D.QTY_DISP) AS AVG_QTY_SOLD
FROM SALES_ORDER S, SALES_ORDER_DETAILS D
WHERE D.PRODUCT_RATE > 15000
GROUP BY S.CLIENT_NO;

--queries using date 
--41)Display the order number and day o which clients placed their order.

SELECT S_ORDER_NO, TO_CHAR(S_ORDER_DATE, 'Day') AS order_day
FROM sales_order;

--42)Display the month (in alphabets) and date when the order must deliver.
SELECT TO_CHAR(DELY_DATE, 'Month') AS delivery_month, TO_CHAR(DELY_DATE, 'DD') AS delivery_day FROM SALES_ORDER;

--43)Display the s_order_date in the format ‘DD-MM-YY’. E.g. 12-February-1996 
SELECT TO_CHAR(S_ORDER_DATE,'DD-MM-YY') AS FORMATED_DATE FROM SALES_ORDER;

--44)Find the date, 15 days after today’s date.
SELECT SYSDATE + 15 FROM DUAL;

--45)Find the number of days elapsed between today’s date and the delivery date of the orders placed by the clients.
SELECT S.S_ORDER_NO,
       TO_DATE(S.DELY_DATE, 'YYYY-MM-DD') AS delivery_date,
       TO_DATE(SYSDATE, 'YYYY-MM-DD') AS current_date,
       TO_DATE(S.DELY_DATE, 'YYYY-MM-DD') - TO_DATE(SYSDATE, 'YYYY-MM-DD') AS days_elapsed FROM SALES_ORDER S;

--TABLE UPDATION
--46)Change the s_order_date of client_no ‘C00001’ to 24/07/96.
UPDATE SALES_ORDER SET S_ORDER_DATE = TO_DATE('26/07/96', 'DD/MM/YY')
WHERE CLIENT_NO = 'C00001';

--47)	Change the selling price of ‘1.44 Floppy Drive’ to Rs. 1150.00
UPDATE product_master SET SELL_PRICE = 1150.00 WHERE DESCRIPTION = '1.44 Floppy Drive';

--48)	Delete the records with order number ‘O19001’ from the order table.
DELETE FROM sales_order WHERE s_order_no = 'O19001';


--49)Delete all the records having delivery date before 10th July’96
DELETE FROM sales_order WHERE dely_date < TO_DATE('1996-07-10', 'YYYY-MM-DD');

--50)Change the city of client_no ‘C00005’ to ’Bombay’.
UPDATE client_master SET city = 'Bombay' WHERE client_no = 'C00005';

--51)Change the delivery date of order number ‘O10008” to 16/08/96
UPDATE Sales_order SET dely_date = TO_DATE('16/08/96', 'DD/MM/YY') WHERE s_order_no = 'O10008';

--52)Change the bal_due of client_no ‘C00001’ to 1000
UPDATE Client_master SET bal_due = 1000 WHERE client_no = 'C00001';

--53)Change the cost price of ‘1.22 Floppy Drive’ to Rs. 950.00.
UPDATE Product_master SET cost_price = 950.00 WHERE description = '1.22 Floppy Drive';
