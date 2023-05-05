# New-Wheels
New-Wheels sales have been dipping steadily in the past year, customer feedback and ratings online have been critical, there has been a drop in new customers every quarter, which is concerning to the business. The CEO of the company now wants a quarterly report with all the key metrics sent to access business health



-----------------------------------------------------------------------------------------------------------------------------------

											Database Creation
                                               
-----------------------------------------------------------------------------------------------------------------------------------
*/
-- Create Database Vehdb

DROP DATABASE IF EXISTS vehdb;
CREATE DATABASE vehdb;




-- [2] Now, after creating the database, you need to tell MYSQL which database is to be used.

USE vehdb;

/*-----------------------------------------------------------------------------------------------------------------------------------

                                               Tables Creation
                                               
-----------------------------------------------------------------------------------------------------------------------------------*/

-- [3] Creating the tables:

-- Table Creation for Temp_t

CREATE TABLE temp_t(
	shipper_id INTEGER,
	shipper_name VARCHAR(25),
	shipper_contact_details VARCHAR(20),
	product_id INTEGER,
	vehicle_maker VARCHAR(25),
	vehicle_model VARCHAR(25),
	vehicle_color VARCHAR(20),
	vehicle_model_year INTEGER,
	vehicle_price DECIMAL(10,2),
	quantity INTEGER,
	discount DECIMAL(4,2),
	customer_id VARCHAR(20),
	customer_name VARCHAR(30),
	gender VARCHAR(15),
	job_title VARCHAR(50),
	phone_number VARCHAR(15),
	email_address VARCHAR(50),
	city VARCHAR(25),
	country VARCHAR(20),
	state VARCHAR(25),
	customer_address VARCHAR(100),
	order_date DATE,
	order_id VARCHAR(20),
	ship_date DATE,
	ship_mode VARCHAR(20),
	shipping VARCHAR(15),
	postal_code INTEGER,
	credit_card_type VARCHAR(35),
	credit_card_number BIGINT,
	customer_feedback VARCHAR(20),
	quarter_number INTEGER,
PRIMARY KEY(shipper_id, product_id, customer_id, order_id)
     );

-- Table Creation for Vehicle_t

CREATE TABLE vhb_vehicle_t(
	shipper_id INTEGER,
	shipper_name VARCHAR(25),
	shipper_contact_details VARCHAR(20),
	product_id INTEGER,
	vehicle_maker VARCHAR(25),
	vehicle_model VARCHAR(25),
	vehicle_color VARCHAR(20),
	vehicle_model_year INTEGER,
	vehicle_price DECIMAL(7,2),
	quantity INTEGER,
	discount DECIMAL(4,2),
	customer_id VARCHAR(20),
	customer_name VARCHAR(30),
	gender VARCHAR(15),
	job_title VARCHAR(50),
	phone_number VARCHAR(15),
	email_address VARCHAR(50),
	city VARCHAR(25),
	country VARCHAR(20),
	state VARCHAR(25),
	customer_address VARCHAR(100),
	order_date DATE,
	order_id VARCHAR(20),
	ship_date DATE,
	ship_mode VARCHAR(20),
	shipping VARCHAR(15),
	postal_code INTEGER,
	credit_card_type VARCHAR(35),
	credit_card_number BIGINT,
	customer_feedback VARCHAR(20),
	quarter_number INTEGER,
PRIMARY KEY(shipper_id, product_id, customer_id, order_id)
     );
     
-- Table Creation for product_t

CREATE TABLE vhb_product_t(
	product_id INTEGER,
    vehicle_maker VARCHAR(25),
	vehicle_model VARCHAR(25),
	vehicle_color VARCHAR(20),
	vehicle_model_year INTEGER,
	vehicle_price DECIMAL(7,2),

	PRIMARY KEY(product_id)

);

-- Table Creation for cust_t

CREATE TABLE vhb_cust_t(
	customer_id VARCHAR(20),
    customer_name VARCHAR(30),
	gender VARCHAR(10),
	job_title VARCHAR(50),
	phone_number VARCHAR(15),
	email_address VARCHAR(50),
	city VARCHAR(25),
	country VARCHAR(20),
	state VARCHAR(25),
	customer_address VARCHAR(100),
    postal_code INTEGER,
	discount DECIMAL(4,2),
	credit_card_type VARCHAR(35),
	credit_card_number BIGINT,

PRIMARY KEY(customer_id)
);

-- Table Creation for ord_t

CREATE TABLE vhb_ord_t(
order_id VARCHAR(20),
shipper_id INTEGER,
customer_id VARCHAR(20),
product_id INTEGER,
order_date DATE,
ship_date DATE,
ship_mode VARCHAR(20),
shipping VARCHAR(15),
discount DECIMAL(4,2),
vehicle_price DECIMAL(10,2),
quantity INTEGER,
customer_feedback VARCHAR(20),
quarter_number INTEGER,

PRIMARY KEY(order_id)

);

-- Table Creation for shipper_t

CREATE TABLE vhb_shipper_t(
	shipper_id INTEGER,
    shipper_name VARCHAR(25),
	shipper_contact_details VARCHAR(20),
	PRIMARY KEY(shipper_id)

);

-- To drop table if already exists- 

DROP TABLE IF EXISTS table_name;

/*-----------------------------------------------------------------------------------------------------------------------------------

                                               Stored Procedures Creation
                                               
-----------------------------------------------------------------------------------------------------------------------------------*/

-- [4] Creating the Stored Procedures:

-- Stored Procedure for cust_p
USE vehdb;
DELIMITER $$
CREATE PROCEDURE vhb_cust_p()
BEGIN
INSERT INTO vhb_cust_t (
	customer_id,
    customer_name,     
	gender,
	job_title,
	phone_number,
	email_address,
	city,
	country,
	state,
	customer_address,
    postal_code,
	credit_card_type,
	credit_card_number

) 
SELECT DISTINCT
	customer_id,
	customer_name,     
	gender,
	job_title,
	phone_number,
	email_address,
	city,
	country,
	state,
	customer_address,
    postal_code,
	credit_card_type,
	credit_card_number

FROM vehb_vehicle_t
WHERE customer_id NOT IN (select distinct customer_id FROM vhb_cust_t);
END;

CALL vhb_cust_p();

-- Stored Procedure for vehicle_p

USE vehdb;
DELIMITER $$
CREATE PROCEDURE vhb_vehicle_p()
BEGIN
INSERT INTO vehb_vehicle_t (
	shipper_id ,
	shipper_name,
	shipper_contact_details,
	product_id,
	vehicle_maker,
	vehicle_model,
	vehicle_color,
	vehicle_model_year,
	vehicle_price,
	quantity,
	discount,
	customer_id,
	customer_name,
	gender,
	job_title,
	phone_number,
	email_address,
	city,
	country,
	state,
	customer_address,
	order_date,
	order_id,
	ship_date ,
	ship_mode,
	shipping,
	postal_code,
	credit_card_type,
	credit_card_number,
	customer_feedback,
	quarter_number
    )
    SELECT * FROM temp_t;
    END;
    
    CALL vhb_vehicle_p;
    
 -- Stored Procedure for ord_p

USE vehdb;
DELIMITER $$
CREATE PROCEDURE vhb_ord_p(quarter_num integer)
BEGIN
INSERT INTO vhb_ord_t (
	order_id,
    shipper_id,
	customer_id,
    product_id,     
	order_date,
	ship_date,
	ship_mode,
	shipping,
	discount,
	vehicle_price,
	quantity,
	customer_feedback,
	quarter_number 

) 
SELECT DISTINCT
	order_id,
	shipper_id,
	customer_id,
    product_id,     
	order_date,
	ship_date,
	ship_mode,
	shipping,
	discount,
	vehicle_price,
	quantity,
	customer_feedback,
	quarter_number 

FROM vehb_vehicle_t
WHERE order_id NOT IN (select distinct order_id FROM vhb_ord_t);
END;

CALL vhb_ord_p(4);

 -- Stored Procedure for product_p
 
 USE vehdb;
DELIMITER $$
CREATE PROCEDURE vhb_product_p()
BEGIN
INSERT INTO vhb_product_t (
	product_id,
    vehicle_maker,
	vehicle_model,
	vehicle_color,
	vehicle_model_year,
	vehicle_price


) 
SELECT DISTINCT
	product_id,
    vehicle_maker,
	vehicle_model,
	vehicle_color,
	vehicle_model_year,
	vehicle_price
FROM vehb_vehicle_t
WHERE product_id NOT IN (select distinct product_id FROM vhb_product_t);
END;

CALL vhb_product_p();

-- Stored Procedure for shipper_p
USE vehdb;
DELIMITER $$
CREATE PROCEDURE vhb_shipper_p()
BEGIN
INSERT INTO vhb_shipper_t (
	shipper_id,
    shipper_name,
    shipper_contact_details

) 
SELECT DISTINCT
	shipper_id,
    shipper_name,
    shipper_contact_details
FROM vehb_vehicle_t
WHERE shipper_id NOT IN (select distinct shipper_id FROM vhb_shipper_t);
END;

CALL vhb_shipper_p();



    -- To drop the stored procedure if already exists- 
DROP PROCEDURE IF EXISTS procedure_name;


/*-----------------------------------------------------------------------------------------------------------------------------------

                                               Data Ingestion
                                               
-----------------------------------------------------------------------------------------------------------------------------------*/

-- [5] Ingesting the data:

TRUNCATE temp_t;
LOAD DATA INFILE "C://ProgramData//MySQL//MySQL Server 8.0//Uploads//new_wheels_proj//Data//new_wheels_sales_qtr_4.csv"

INTO TABLE temp_t
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;

/*call vehicles_p();
call customer_p();
call product_p();
call shipper_p();
call order_p();*/


/*-----------------------------------------------------------------------------------------------------------------------------------

                                               Views Creation
                                               
-----------------------------------------------------------------------------------------------------------------------------------*/

-- [6] Creating the views:
-- View Creation for veh_ord_cust_v

CREATE VIEW veh_ord_cust_v AS
SELECT o.order_id,
	   o.shipper_id,
       o.customer_id,
       o. product_id,
	   o.order_date,
	   o.ship_date,
       o.ship_mode,
       o.shipping,
	   o.discount,
	   o.vehicle_price,
	   o.quantity,
       o.customer_feedback,
       o.quarter_number,
       c.customer_name,
       c.gender,
       c.job_title,
       c.phone_number,
       c.email_address,
	   c.city,
	   c.country,
       c.state,
       c.customer_address,
       c.postal_code,
	   c.credit_card_type,
	   c.credit_card_number
FROM vhb_ord_t AS o 	
INNER JOIN vhb_cust_t AS c
ON o.customer_id= c.customer_id;

-- View Creation for veh_prod_cust_v

CREATE VIEW veh_prod_cust_v AS
SELECT c.customer_name,
       c.gender,
       c.job_title,
       c.phone_number,
       c.email_address,
	   c.city,
	   c.country,
       c.state,
       c.customer_address,
       c.postal_code,
	   c.credit_card_type,
	c.credit_card_number,
    p.product_id,
    p.vehicle_maker,
    p.vehicle_model,
    p.vehicle_color,
    p.vehicle_model_year,
    p.vehicle_price
FROM vhb_ord_t AS o
INNER JOIN vhb_cust_t AS c
ON o.customer_id= c.customer_id
INNER JOIN vhb_product_t p
ON o.product_id= p.product_id;


-- To drop the views if already exists- 
DROP VIEW IF EXISTS view_name;




/*-----------------------------------------------------------------------------------------------------------------------------------

                                               Functions Creation
                                               
-----------------------------------------------------------------------------------------------------------------------------------*/

-- [7] Creating the functions:


-- Create the function calc_revenue_f

DELIMITER $$  
CREATE FUNCTION calc_revenue_f (vehicle_price DECIMAL(10,2), quantity INT, discount DECIMAL(4,2)) 
RETURNS DECIMAL
DETERMINISTIC  
BEGIN  
	DECLARE revenue DECIMAL;
    SET revenue= vehicle_price*quantity*discount;
    Return (revenue);
END;


-- Create the function days_to_ship_f-

DELIMITER $$
CREATE FUNCTION days_to_ship_f ( ship_date DATE, order_date DATE) 
RETURNS INTEGER
DETERMINISTIC
BEGIN 
DECLARE days INTEGER;
SET days=DATEDIFF( ship_date, order_date);
RETURN (days);


END;

/*-----------------------------------------------------------------------------------------------------------------------------------

  
  
-----------------------------------------------------------------------------------------------------------------------------------

                                                         Queries
                                               
-----------------------------------------------------------------------------------------------------------------------------------*/
  
/*-- QUESTIONS RELATED TO CUSTOMERS
     [Q1] What is the distribution of customers across states?
     Hint: For each state, count the number of customers.*/

SELECT COUNT(customer_id) AS customer_count,
	   state
FROM veh_ord_cust_v
GROUP BY state;



-- ---------------------------------------------------------------------------------------------------------------------------------

/* [Q2] What is the average rating in each quarter?
-- Very Bad is 1, Bad is 2, Okay is 3, Good is 4, Very Good is 5.
*/
WITH average_rating as 
(
SELECT
		customer_id,
        quarter_number,
        CASE 
			WHEN customer_feedback='Very Bad' THEN 1
			WHEN customer_feedback='Bad' THEN 2
			WHEN customer_feedback='Okay' THEN 3
			WHEN customer_feedback='Good' THEN 4
			WHEN customer_feedback='Very Good' THEN 5
        END AS rating
FROM veh_ord_cust_v
)
SELECT AVG(rating) AS avgrating, 
		quarter_number

FROM average_rating 
GROUP BY quarter_number
ORDER BY quarter_number;



-- ---------------------------------------------------------------------------------------------------------------------------------

/* [Q3] Are customers getting more dissatisfied over time?

Hint: Need the percentage of different types of customer feedback in each quarter. Use a common table expression and
	  determine the number of customer feedback in each category as well as the total number of customer feedback in each quarter.
	  Now use that common table expression to find out the percentage of different types of customer feedback in each quarter.
      Eg: (total number of very good feedback/total customer feedback)* 100 gives you the percentage of very good feedback.
      
Note: For reference, refer to question number 10. Week-2: Hands-on (Practice)-GL_EATS_PRACTICE_EXERCISE_SOLUTION.SQL. 
      You'll get an overview of how to use common table expressions from this question*/
      
WITH feedback_bucket AS
(
	SELECT
    customer_id,
    quarter_number,
        CASE 
			WHEN customer_feedback='Very Bad' THEN 1
			WHEN customer_feedback='Bad' THEN 2
			WHEN customer_feedback='Okay' THEN 3
			WHEN customer_feedback='Good' THEN 4
			WHEN customer_feedback='Very Good' THEN 5
		END AS feedback
    FROM  veh_ord_cust_v 
)
SELECT SUM(feedback) AS total_feedback,
	   COUNT(feedback),
		quarter_number
FROM feedback_bucket
GROUP BY 3;




-- ---------------------------------------------------------------------------------------------------------------------------------

/*[Q4] Which are the top 5 vehicle makers preferred by the customer.

*/

SELECT SUM(quantity) AS VehMktotal,
	    vehicle_maker
		FROM veh_ord_cust_v AS o
        INNER JOIN veh_prod_cust_v AS p
        ON o.product_id=p.product_id
GROUP BY vehicle_maker
ORDER BY VehMktotal DESC
LIMIT 5;




-- ---------------------------------------------------------------------------------------------------------------------------------

/*[Q5] What is the most preferred vehicle make in each state?

Hint: Use the window function RANK() to rank based on the count of customers for each state and vehicle maker. 
After ranking, take the vehicle maker whose rank is 1.*/

SELECT COUNT(customer_id) AS PVehMk,
	   p.vehicle_maker,
	   o.state,
	   RANK() OVER(PARTITION BY o.state
	   ORDER BY vehicle_maker ASC )PVehMkRank
FROM veh_ord_cust_v o
  INNER JOIN veh_prod_cust_v AS p
		ON o.product_id=p.product_id
GROUP BY o.state;
		
-- ---------------------------------------------------------------------------------------------------------------------------------

/*QUESTIONS RELATED TO REVENUE and ORDERS 

-- [Q6] What is the trend of number of orders by quarters?
*/

SELECT COUNT(order_id) AS NofOrders, 
	   quarter_number
FROM veh_ord_cust_v
GROUP BY quarter_number;



-- ---------------------------------------------------------------------------------------------------------------------------------

/* [Q7] What is the quarter over quarter % change in revenue? 

.*/
      
 WITH QoQ AS 
(
	SELECT
		quarter_number,
		SUM(calc_revenue_f(vehicle_price, quantity,discount))AS revenue
	FROM veh_ord_cust_v
		
	GROUP BY 1
)
SELECT
quarter_number,
    revenue,
    LAG(revenue) OVER (ORDER BY quarter_number) AS previous_quarter_revenue,
    ((revenue - LAG(revenue) OVER (ORDER BY quarter_number))/LAG(revenue) OVER(ORDER BY quarter_number) * 100) AS "quarter_over_quarter_revenue(%)"
FROM
	QOQ;
         
      
      
      

-- ---------------------------------------------------------------------------------------------------------------------------------

/* [Q8] What is the trend of revenue and orders by quarters?

*/

SELECT  COUNT(order_id) total_orders,
	    SUM(calc_revenue_f(vehicle_price, quantity,discount)) AS revenue,
        quarter_number
FROM veh_ord_cust_v
GROUP BY  quarter_number
ORDER BY quarter_number;




                           
-- ---------------------------------------------------------------------------------------------------------------------------------

/* Questions related to shipping
   What is the average discount offered for different types of credit cards?
*/

SELECT AVG(discount) AS avgdiscount, 
	   credit_card_type
FROM veh_ord_cust_v
GROUP BY credit_card_type;


-- ---------------------------------------------------------------------------------------------------------------------------------

/* [Q10] What is the average time taken to ship the placed orders for each quarters?
   Use days_to_ship_f function to compute the time taken to ship the orders.
*/

SELECT quarter_number,
	   AVG(days_to_ship_f(ship_date, order_date)) AS days
       FROM veh_ord_cust_v
GROUP BY quarter_number
ORDER BY quarter_number;



-- --------------------------------------------------------Done----------------------------------------------------------------------
-- ----------------------------------------------------------------------------------------------------------------------------------
