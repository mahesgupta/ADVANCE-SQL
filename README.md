# ADVANCE SQL ASSIGNMENT
 
     This is my AdvanceSQL Assignment

     Below I have explain the approach and shown the output screenshot to my solution:
     
###  Question 1:  Write a query that gives an overview of how many films have replacements costs in the following cost ranges
###               low: 9.99 - 19.99
###               medium: 20.00 - 24.99
###               high: 25.00 - 29.99
###               Solution:-  
                           SELECT SUM(CASE WHEN replacement_cost BETWEEN 9.99 AND 19.99 THEN 1 ELSE 0 END) AS low,  
                           SUM(CASE WHEN replacement_cost BETWEEN 20.00 AND 24.99 THEN 1 ELSE 0 END) AS medium,  
                           SUM(CASE WHEN replacement_cost BETWEEN 25.00 AND 29.99 THEN 1 ELSE 0 END) AS high  
                           FROM film; 
###      Approach:-

1. Here in this particular question we have been asked to give the overview of the films where the replacement_cost is in the given range categorizing it into low,medium and high.  
2. So I have used window function where I have used CASE statement for filtering the data according to the given range of replacement cost and i categorize those filtered data for each case statement as low,medium and high  
3. Logic in the case statement is if the particular replacement_cost is within the specified range for the particular category then returning 1 else 0 which we are summing up using SUM() function which results in total number of films in that particular range                       
                           


<img width="507" alt="Screenshot 2023-03-07 at 11 23 16 PM" src="https://user-images.githubusercontent.com/122514232/223523840-66cbd96f-5f24-4fb3-9ee2-0fa327f9ff59.png">

 
 
 
 ###   Question 2:
###    Write a query to create a list of the film titles including their film title, film length and film category name ordered descendingly by the film length. Filter the results to only the movies in the category 'Drama' or 'Sports'.
###     "STAR OPERATION" "Sports" 181
###     "JACKET FRISCO" "Drama" 181
###   Solution:-


SELECT F.title, C.name, F.length
FROM film  F INNER JOIN film_category FC
ON F.film_id=FC.film_id
INNER JOIN CATEGORY C
ON C.category_id=FC.category_id
WHERE C.name='Sports' or C.name='Drama'
ORDER BY F.length;



###      Approach:-

1. Here we have to create a list of the film titles including their film title, film length and film category name ordered descendingly by the film length for the category “sports” and “drama”
2. So for that I have joined three tables i.e., film, film_category, category and extracted the film_title, film_length and film_category by applying the filter for category name to be only “Sports” and “Drama” ordering by the length of the film in descending order.

<img width="498" alt="Screenshot 2023-03-08 at 12 28 10 AM" src="https://user-images.githubusercontent.com/122514232/223523649-053f00df-5612-43e1-b5ab-d2056a3fd1f1.png">  



###   Question 3:
###          Write a query to create a list of the addresses that are not associated to any customer.
###          Solution:-
               SELECT * 
               FROM address
               WHERE address_id NOT IN (SELECT address_id FROM customer);   
               
###      Approach:-
1. We have been asked to create a list of the addresses that are not associated to any customer.  

2. So For that I have used two tables i.e., address and customer to extract address_id, address, district, city_id and customer_id. 
3. I have used left join above to retrieve all the data from the address table irrespective of whether it is present in the customer table. So whichever customer_id is not associated with any address will result in null values which i am using in the where clause to filter out the data where customer_id is null which will give me all the address which are not related to any customer.
               
<img width="625" alt="Screenshot 2023-03-08 at 12 31 23 AM" src="https://user-images.githubusercontent.com/122514232/223524544-9cc68327-06b0-4fcb-ab6f-d9077f8c9a18.png">


###       Question 4:
###       Write a query to create a list of the revenue (sum of amount) grouped by a column in the format "country, city" ordered in decreasing amount of revenue.
###       eg. "Poland, Bydgoszcz" 52.88
###       Solution:-  
                  
               SELECT CO.country, CI.city , SUM(P.amount)
               FROM country CO,city CI ,address A,customer C ,payment P
               WHERE CO.country_id=CI.country_id AND CI.city_id=A.city_id AND A.address_id=C.address_id AND C.customer_id=P.customer_id
               GROUP BY CO.country,CI.city
               ORDER BY SUM(P.amount) DESC;
               
###             Approach:-

1. Here we need to create a list of the revenue based on country, city according to decreasing amount of revenue.  
2. So for getting the desired result I have joined 4 tables i.e., country, city, address and payment to fetch country name, city name which I am concatenating using CONCAT() function.  
3. Also calculated the revenue by using window function which is calculating the sum of amount partitioning by Country,City and then ordering revenue by descending order .  

<img width="625" alt="Screenshot 2023-03-08 at 12 36 20 AM" src="https://user-images.githubusercontent.com/122514232/223525912-90a329f4-499e-403c-b23e-dcc114e2f654.png">



###     Question 5:
###     Write a query to create a list with the average of the sales amount each staff_id has per customer.
###     result: 
###             2 56.64
###             1 55.91
###     Solution:-
        
              SELECT C.staff_id, ROUND(AVG(S),2)
              FROM
             (SELECT S.staff_id,sum(P.amount) AS S
             FROM staff s, payment P
             where S.staff_id=P.staff_id
             GROUP BY S.staff_id,P.customer_id) C
             GROUP BY C.staff_id;  
             
 ###      Approach:-             
1. Here we are asked to create a list with the average of sales per customer with each staff id.  
2. Here each customer has multiple transaction with particular staff id, so first we need to find total amount of transaction each customer had with particular staff id, so I am calculating that In the derived table (d) above using window function which is calculating the sum of amount partitioning by staff_id,customer_id.  
3. Now using derived table I am extracting staff_id and calculating average of the total_amount per customer calculated in derived table to find out the average amount of sales each staff had.  
4. Now using derived table I am extracting staff_id and calculating average of the total_amount per customer calculated in derived table to find out the average amount of sales each staff had.  


<img width="625" alt="Screenshot 2023-03-08 at 12 40 48 AM" src="https://user-images.githubusercontent.com/122514232/223526878-a29c977a-81f1-4fb8-8f65-0e67bd4c82cb.png">


###     Question 6:
###     Write a query that shows average daily revenue of all Sundays.
###     Solution:-
                    SELECT AVG(D.AMOUNT) from
                   (SELECT C.DATE_ONLY ,SUM(C.amount) AS AMOUNT
                   FROM 
                   (SELECT date(payment_date) AS DATE_ONLY, amount
                   FROM payment
                   where WEEKDAY(payment_date)=6) C
                   GROUP BY C.DATE_ONLY ) D;
                   
                   
  ###                Approach:-

1. We have been asked to show average daily revenue of all Sundays.  
2. So for that I have used single payment table wherein in the inner derived table I am extracting the payment_date and amount by filtering the data where the date is of sunday by using DAYNAME() function in where clause.  
3. In each sunday their can be multiple transaction so in order to find out the average, first we need to calculate the total of all the transaction in every sunday.So in outer derived table I am calculating the sum of amount grouping by the date (obtain from inner derived table   by filtering out on the basis of sunday) which will give me the total transaction in all sundays.  
4. Now that I have got total amount ,so I can find the average of that total amount which will give me the desired result that is average daily revenue of all sundays.  



<img width="628" alt="Screenshot 2023-03-08 at 12 47 00 AM" src="https://user-images.githubusercontent.com/122514232/223528145-403e50d3-9a67-41b2-9e74-24e044183423.png">


###         Question 7:
###         Write a query to create a list that shows how much the average customer spent in total (customer life-time value) grouped by the different                 districts.
###         Solution:-
                   SELECT B.district,AVG(B.total_payment_per_customer)
                   FROM
                   (SELECT A.district,C.customer_id ,SUM(P.amount) AS total_payment_per_customer
                   FROM address A , customer C , payment P
                   where A.address_id=C.address_id AND C.customer_id=P.customer_id
                   GROUP BY A.district,C.customer_id
                    ) B
                   GROUP BY B.district;
                   
###                Approach:-                  
1. Here we need to find out the average amount of all customer spent in total based on each district
2. For that I have used three tables i.e., customer, address and payment and extracted customer_id, district and the total amount for each customer as        each customer have multiple transaction in each district
3. Now making above as derived table I extracted district and Average of total_sum calculated for each customer in the derived table partitioning by the district.So this way I am getting the district and the average of customers total spent grouped by each district
                   
<img width="628" alt="Screenshot 2023-03-08 at 12 50 56 AM" src="https://user-images.githubusercontent.com/122514232/223529047-af94213d-f187-4e71-a403-bbfbb2903924.png">



###         Question 8:
###         Write a query to list down the highest overall revenue collected (sum of amount per title) by a film in each category. Result should display               the film title, category name and total revenue.
###         eg. "FOOL MOCKINGBIRD" "Action" 175.77
###         "DOGMA FAMILY" "Animation" 178.7
###         "BACKLASH UNDEFEATED" "Children" 158.81
###          Solution:-
                    select * from film;
                    select N.title,N.CATEG_NAME,N.total_collection 
                    from
                    (SELECT M.title,M.CATEG_NAME,M.total_collection, MAX(total_collection) OVER(PARTITION BY M.CATEG_NAME) MAXIMUM_OVER_CATEG
                    FROM
                    (select  F.title ,C.name AS CATEG_NAME ,sum(P.amount) AS total_collection
                    from film F, inventory I , rental R, payment P, film_category FC, category C
                    where F.film_id=I.film_id 
                    AND I.inventory_id=R.inventory_id 
                    AND  R.rental_id=P.rental_id 
                    AND F.film_id=FC.film_id 
                    AND FC.category_id=C.category_id
                    GROUP BY F.title,C.name ) M
                    ) N
                   WHERE N.total_collection=N.MAXIMUM_OVER_CATEG;    
                   
                   
  ###      Approach:-                 
1. Here we need to list out the film_title and highest overall revenue collected sum of amount per title by a film for each category For this I have used    6 tables i.e., film, film_category ,category, inventory, rental, payment and from that I have extracted film_title, film category and sum of amount        partition by film_title, so this will give me the overall revenue for each film title
2. Now from inner derived table I have the data of film_title, film category and overall revenue generated from each film.So in outer derived table I       extracted the film_title, category and Maximum of overall revenue for each category by using window function for finding MAX of overall_revenue           partition by category.
3. Now from outer derived table I extracted the film_title,category name and the max revenue where my  max_revenue= overall_revenue. This way I am getting   those film title who has generated highest revenue in each category.


<img width="628" alt="Screenshot 2023-03-08 at 12 55 06 AM" src="https://user-images.githubusercontent.com/122514232/223531406-697b75f9-0b43-4264-b061-9b393fd24250.png">


###   Question 9:
###   Modify the table "rental" to be partitioned using PARTITION command based on ‘rental_date’ in below intervals:
	<2005
        between 2005–2010
	between 2011–2015
	between 2016–2020
	>2020 - Partitions are created yearly
###   Solution:-
              ALTER TABLE rental 
              PARTITION BY RANGE(YEAR(rental_date))
              (
              PARTITION rental_less_than_2005 VALUES LESS THAN (2005),
              PARTITION rental_between_2005_2010 VALUES LESS THAN (2011),
              PARTITION rental_between_2011_2015 VALUES LESS THAN (2016),
              PARTITION rental_between_2016_2020 VALUES LESS THAN (2021),
              PARTITION rental_greater_than_2020 VALUES LESS THAN MAXVALUE
               );
               
               
###               Approach:-

1. Here In this particular question first I have to drop the foreign keys as partitioning was not supporting the foreign key constraints in MYSQL as it can change the underlying structure of the table.     
2. Next I partitioned the rental table by rental_date with the specified range of years.  
3. First Partition “rental_less_than_2005” will contain those values where year is less than 2005.  
4. Second Partition “rental_between_2005_2010” will contain those values where year is between 2005 and 2010.  
5. Third Partition “rental_between_2011_2015” will contain those values where year is between 2011 and 2015.  
6. Fourth Partition “rental_between_2016_2020” will contain those values where year is between 2016 and 2020.  
7. And everything after 2020 will be stored in the partition “rental_greater_than_2020”.  

     
     
 ###        Question 10:
###         Modify the table "film" to be partitioned using PARTITION command based on ‘rating’ from below list. Further apply hash sub-partitioning based             on ‘film_id’ into 4 sub-partitions.
###         partition_1 - "R"
###         partition_2 - "PG-13", "PG"
###         partition_3 - "G", "NC-17"
###         Solution:-
                     
                     ALTER TABLE film
                     PARTITION BY LIST(rating)
                     SUBPARTITION BY HASH(film_id) SUBPARTITIONS 4
                     (
                     PARTITION PR values('R'),
                     PARTITION Pgs values('PG-13', 'PG'),
                     PARTITION GNC values('G', 'NC-17')
                     );
                   
###                   Approach:-

1. Here first I have used PARTITION BY LIST based on rating.
2. Then I have used SUBPARTITION BY HASH based on fil.


###       Question 11:
###              Write a query to count the total number of addresses from the “address” table where the ‘postal_code’ is of the below formats. Use                        regular expression.
###              9*1**, 9*2**, 9*3**, 9*4**, 9*5**
###      Solution:-
                 SELECT count(postal_code) 
                 FROM address
                 WHERE postal_code REGEXP '^9[0-9][1-5][0-9]{2}';  -- Using regular expression to retrieve all the postal code with the givrn pattern

 ###                  Approach:-

1.Here I have used the address table to extract postal_code of the specific pattern by using a regular expression in the where clause to filter out the data.  
2. Next I used the Aggregate function COUNT to count the number of postal code following the specific pattern.  

<img width="628" alt="Screenshot 2023-03-08 at 1 07 53 AM" src="https://user-images.githubusercontent.com/122514232/223534147-57b9b360-a370-4042-abad-936eb22126bc.png">



###              Question 12:
###                      Write a query to create a materialized view from the “payment” table where ‘amount’ is between(inclusive) $5 to $8. The view                              should manually refresh on demand. Also write a query to manually refresh the created materialized view.
###             Solution:-
                        DELIMITER $$
                        CREATE EVENT refresh_payment_between_5_8      -- Creating the event which refresh the view every day.
                        ON SCHEDULE EVERY 1 DAY
                        DO
                        BEGIN
                        CREATE OR REPLACE VIEW payment_between_5_8 AS
                        SELECT *
                        FROM payment
                        WHERE amount BETWEEN 5 AND 8;
                        END$$
                        DELIMITER ;

                        -- APPLYING SELECT CLAUSE IN VIEW CREATED ABOVE WHICH CONTAINS THE DATA WHERE AMOUNT IS BEWTWEEN $5 AND $8
                       SELECT * FROM payment_between_5_8;
                       
                       
                       
###                   Approach:-

1. As materialized view was not supported by MYSQL so I have created the event name “refresh_payment_between_5_8”
which is scheduled to refresh in every one day and inside that I have created the normal view which stores all 
the data of the payment table where amount is between $5 and $ 8.

<img width="625" alt="Screenshot 2023-03-08 at 1 12 30 AM" src="https://user-images.githubusercontent.com/122514232/223535188-26f9b9dc-6651-439c-82e8-0774df139c1c.png">



###          Question 13:
###          Write a query to list down the total sales of each staff with each customer from the ‘payment’ table. In the same result, list down the                    total sales of each staff i.e. sum of sales from all customers for a particular staff. Use the ROLLUP command. Also use GROUPING command to                indicate null values.
###          Solution:-
                     SELECT p.staff_id,p.customer_id,
                     GROUPING(p.staff_id) as staff,
                     GROUPING(p.customer_id) as customer,sum(p.amount) as sum_of_sales
                     FROM payment p
                     GROUP BY p.staff_id,p.customer_id
                     WITH ROLLUP;
                     
 ###        Approach:-

1. Here we have been asked to list down all the total sales of each staff with each customer. Also we need list down the total sales of each staff i.e. sum of sales from all customers for a particular staff.
2. So for that I have used the payment table to extract staff_id,customer_id and applied grouping on both customer_id and staff_id to handle null values and clearly indicate the ROLLUP sum for the particular column
            
 <img width="625" alt="Screenshot 2023-03-08 at 1 15 36 AM" src="https://user-images.githubusercontent.com/122514232/223535708-c6b36a70-12b8-4d51-a2fc-c2355389c53a.png">
 
 
###       Question 14:
###                Write a single query to display the customer_id, staff_id, payment_id, amount, amount on immediately previous payment_id, amount on                        immediately next payment_id ny_sales for the payments from customer_id ‘269’ to staff_id ‘1’.
###         Solution:-
                   select customer_id,payment_id,staff_id,
                   lead(amount) over(order by payment_id) next_payment,
                   lag(amount) over(order by payment_id) previous_amount,
                   lead(amount) over(Partition by customer_id,staff_id ORDER BY payment_id) as ny_sales,
                   lag(amount) over(Partition by customer_id,staff_id ORDER BY payment_id) as py_sales
                   from payment
                   where customer_id=269 and staff_id=1;
                   
                   
 ###                  Approach:-

1. Here we have been asked to display the customer_id, staff_id, payment_id, amount, amount on immediately previous payment_id, amount on immediately next payment_id.  
2. For this we have used the payment table and used the window function LEAD to extract the next payment and LAG to extract the previous payment order by payment_id.  
3. Next we need to find the ny_sales - the amount of the next payment made by the same customer to the same staff member and py sales - the amount of the previous payment made by the same customer to the same staff member.For this as well I have used lead and lag function partitioned by customer_id and staff_id.  
4. Used where clause to filter out the data where customer_id=269 and staff_id=1.  


<img width="625" alt="Screenshot 2023-03-08 at 1 18 39 AM" src="https://user-images.githubusercontent.com/122514232/223536678-e51b7695-d01d-4c3a-bc37-4acb49878bd7.png">









               
               


