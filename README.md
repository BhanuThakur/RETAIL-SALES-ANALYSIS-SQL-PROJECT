Conclusion
This project showcases the practical application of SQL in analyzing retail sales data. By creating and querying a relational database, I explored key aspects of sales performance, customer behavior, and operational insights. The project involved:

Data Retrieval: Efficiently filtering sales records for specific dates, categories, and conditions.
Sales Analysis: Identifying top-selling periods, calculating total sales, and determining the most profitable months.
Performance Metrics: Aggregating data to compute averages, totals, and ranks across categories and timeframes.
Operational Insights: Determining transaction patterns across different shifts to improve scheduling and logistics.


Retail Sales SQL Project
Database Creation

    CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,    
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(20),
    age INT,
    category VARCHAR(20),
    quantity FLOAT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);



View All Data in the Table
    
    SELECT * FROM retail_sales;

1. Retrieve all columns for sales made on "2022-11-05"

        SELECT * FROM retail_sales WHERE sale_date = "2022-11-05";
 
2. Retrieve all transactions where category is 'CLOTHING' and quantity sold is more than 2 in November 2022:

        SELECT * FROM retail_sales WHERE category = "CLOTHING" AND quantity > 2 AND sale_date BETWEEN "2022-11-01" AND "2022-11-30";

3. Calculate total sales and total orders placed for each category:

       SELECT category, COUNT(transactions_id), SUM(total_sale) FROM retail_sales GROUP BY category;

4. Calculate the average age of customers who purchased items from the 'CLOTHING' category:

       SELECT AVG(age) FROM retail_sales WHERE category = 'CLOTHING';

5. Find all transactions where total_sale is greater than 1000:

       SELECT * FROM retail_sales WHERE total_sale > 1000;
   
6. Find total transactions made by each gender in each category:

       SELECT gender, category, COUNT(transactions_id) FROM retail_sales GROUP BY gender, category;

7. Calculate average sale for each month and find the best-selling month in each year:

        SELECT MONTHNAME(sale_date) AS month_name, YEAR(sale_date) AS sale_year, ROUND(AVG(total_sale), 2) AS avg_sale
       FROM retail_sales GROUP BY MONTHNAME(sale_date), YEAR(sale_date) 
       ORDER BY YEAR(sale_date), ROUND(AVG(total_sale), 2) DESC;

        SELECT * FROM (
        SELECT 
        MONTHNAME(sale_date) AS month_name, 
        YEAR(sale_date) AS sale_year, 
        ROUND(AVG(total_sale), 2) AS avg_total_sale,
        RANK() OVER (PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) AS rn
       FROM retail_sales 
       GROUP BY MONTHNAME(sale_date), YEAR(sale_date)
        ) AS t1 WHERE rn = 1;

8. Find the top 5 customers based on the highest total sales:

       SELECT customer_id, SUM(total_sale) FROM retail_sales GROUP BY customer_id ORDER BY SUM(total_sale) DESC LIMIT 5;

9.  Find the number of unique customers who purchased items from each category:

        SELECT category, COUNT(DISTINCT customer_id) FROM retail_sales GROUP BY category;

10. Create shifts and count the number of orders in each shift:

        SELECT 
        COUNT(*) AS total_orders,
        sale_time < "12:00:00" AS morning,
        sale_time BETWEEN "12:00:00" AND "17:00:00" AS afternoon,
        sale_time > "17:00:00" AS evening 
        FROM retail_sales GROUP BY morning, afternoon, evening;

    END OF PROJECT 
