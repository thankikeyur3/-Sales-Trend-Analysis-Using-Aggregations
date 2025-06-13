# -Sales-Trend-Analysis-Using-Aggregations


# Re-import required library and regenerate the SQL script after environment reset
sql_script = """
-- Create the table to match Online Selling.csv structure
CREATE TABLE online_selling (
    order_id INT,
    order_date DATE,
    amount DECIMAL(10,2),
    product_id INT
);

-- Load data from CSV (make sure local_infile is enabled and path is correct)
-- Note: Modify the path below based on your local machine
LOAD DATA LOCAL INFILE 'D:/Tasks/Task 6/Online Selling.csv'
INTO TABLE online_selling
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\\n'
IGNORE 1 ROWS;

-- Query 1: View first 10 rows
SELECT * FROM online_selling LIMIT 10;

-- Query 2: Monthly Revenue
SELECT 
    EXTRACT(YEAR FROM order_date) AS year,
    EXTRACT(MONTH FROM order_date) AS month,
    SUM(amount) AS monthly_revenue
FROM online_selling
GROUP BY year, month
ORDER BY year, month;

-- Query 3: Monthly Order Volume
SELECT 
    EXTRACT(YEAR FROM order_date) AS year,
    EXTRACT(MONTH FROM order_date) AS month,
    COUNT(DISTINCT order_id) AS order_volume
FROM online_selling
GROUP BY year, month
ORDER BY year, month;

-- Query 4: Monthly Revenue + Volume Combined
SELECT 
    EXTRACT(YEAR FROM order_date) AS year,
    EXTRACT(MONTH FROM order_date) AS month,
    SUM(amount) AS monthly_revenue,
    COUNT(DISTINCT order_id) AS order_volume
FROM online_selling
GROUP BY year, month
ORDER BY year, month;

-- Query 5: Top 3 Revenue Months
SELECT 
    EXTRACT(YEAR FROM order_date) AS year,
    EXTRACT(MONTH FROM order_date) AS month,
    SUM(amount) AS revenue
FROM online_selling
GROUP BY year, month
ORDER BY revenue DESC
LIMIT 3;

-- Query 6: Create a View
CREATE VIEW monthly_sales_summary AS
SELECT 
    EXTRACT(YEAR FROM order_date) AS year,
    EXTRACT(MONTH FROM order_date) AS month,
    SUM(amount) AS revenue,
    COUNT(DISTINCT order_id) AS volume
FROM online_selling
GROUP BY year, month;
"""

# Save the SQL script to a file
file_path = "/mnt/data/TASK_6_Sales_Analysis.sql"
with open(file_path, "w") as f:
    f.write(sql_script)

file_path
