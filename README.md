# Credit_Card_Transactions_Data_Analysis_SQL

![image](https://github.com/user-attachments/assets/32b2b489-923a-44a6-8843-290c1648561d)

## 📑 Contents

- [**🌐 Introduction**](#-introduction)
- [**💾 Data Setup in SQL Workbench**](#-data-setup-in-sql-workbench)
- [**🔍 Exploring the Dataset**](#-exploring-the-dataset)
- [**🔍 Analytical Insights**](#-analytical-insights)
- [**⚙️ SQL Techniques**](#️-sql-techniques)
- [**📂 Data Source**](#-data-source)
- [**🛠️ Skills Demonstrated**](#️-skills-demonstrated)




Credit Card Transactions Data Analysis Project: In today’s data-driven world, analyzing transaction data has become a key aspect of understanding consumer behavior and financial patterns. This project aims to delve into the world of credit card transactions by analyzing a dataset containing records from major Indian cities such as Delhi, Greater Mumbai, Bengaluru, and Ahmedabad.

The primary focus of this project is to explore transaction data based on credit card types, cities, and expenditure patterns. The data includes crucial attributes such as transaction ID, city, transaction date, card type (Gold, Silver, Platinum, Signature), expenditure type (Bills), gender, and transaction amount.

Throughout this project, I utilized data analysis techniques to gain insights into spending patterns, city-wise expenditure comparisons, and card type preferences. The project also involves identifying high-value transactions and analyzing monthly spending trends to understand consumer habits better.

For data analysis, I employed SQL for data manipulation and aggregation. I used various SQL functions like SUM, COUNT, GROUP BY, ORDER BY, and RANK to extract meaningful insights. Additionally, I plan to create a visualization dashboard using Power BI or Tableau to make the insights more accessible and engaging.

Through this project, I not only honed my SQL querying skills but also developed a comprehensive understanding of financial transaction data analysis. The project highlights the significance of data-driven decision-making in financial management and consumer analysis.

This experience has reinforced my confidence in handling real-world data and paved the way for tackling similar analytical challenges in the future.

Don't forget to follow and star ⭐ the repository if you find it valuable.

Tools Used🛠️:My Sql Workbench


Data Set📂:[Credit Card Transactions Dataset](https://www.kaggle.com/datasets/thedevastator/analyzing-credit-card-spending-habits-in-india?resource=download)

## 💾 Data Setup in SQL Workbench


In this project, I worked with a credit card transactions dataset using MySQL Workbench. The dataset includes several key attributes that reflect real-world financial transactions in various cities across India.

- transaction_id: Unique identifier for each transaction.

- city: The city where the transaction took place (e.g., Delhi, Greater Mumbai, Bengaluru, Ahmedabad).

- transaction_date: The date of the transaction, formatted as DD-MMM-YY.

- card_type: Type of credit card used, such as Gold, Silver, Platinum, and Signature.

- exp_type: The expenditure type, which is categorized as 'Bills' in this dataset.

- gender: The gender of the cardholder, with only 'F' (female) present in this dataset.

- amount: The transaction amount in INR.

These attributes were queried using SQL to perform data analysis operations, including aggregation, filtering, sorting, and ranking to extract financial insights. The project focused on identifying spending patterns by card type, city-wise expenditure distribution, and monthly transaction trends to provide actionable business insights.

## 🔍 Exploring the Dataset

After setting up the dataset in MySQL Workbench, the initial step was to explore and understand the structure of the data. Using queries like SELECT * FROM table_name LIMIT 5;, I previewed the dataset to get a comprehensive overview of the information available for analysis.

This exploratory phase involved:

Reviewing column names, data types, and sample values to familiarize myself with the dataset structure.

Identifying relationships between key attributes such as transaction_id, city, card_type, and transaction_date.

Assessing the distribution of card types (Gold, Silver, Platinum, Signature) to identify spending patterns.

Analyzing the variety of data across cities to understand regional spending trends.

Checking for missing values, data inconsistencies, and potential outliers in transaction amounts.

This step provided a clear understanding of the dataset and helped in structuring the subsequent analysis queries to extract meaningful financial insights.


```sql

# Viewing the Data
select * from credit_card_transactions;

```
**Note:** The image displays only a partial view of the complete results due to screenshot limitations. For the full dataset and comprehensive tables, please execute these queries directly in your SQL environment or refer to the complete result exports available in this repository's data outputs folder.

![Full Table ](https://github.com/user-attachments/assets/62701530-add0-4d70-aa96-a81fb2a66e84)



## 🔍 Analytical Insights

-  ### **`Top 5 Cities by Credit Card Spend`**
  
1.Write a query to print top 5 cities with highest spends and their percentage contribution of total credit card spends?.

```sql
# Viewing the Data
with cte as(select sum(amount) as total_spent from credit_card_transactions)
select  city ,sum(amount) as expense,total_spent,sum(amount)/total_spent *100 as percentage_contributation
from credit_card_transactions
cross join cte 
group by city,total_spent
order by expense desc
limit 5;

```
![Queston 1](https://github.com/user-attachments/assets/93b533d0-8383-475f-8fdd-98741bebf7d0)

Query Explanation:

The query outputs the top 5 cities with the highest credit card spending, their respective spending amounts, the total spending across all cities, and the percentage contribution of each city to the overall total. This analysis provides valuable insights into the cities with the highest financial activity, helping businesses understand regional spending patterns for targeted marketing and financial planning.

-  ### **`Highest Spend Month by Card Type`**
2. Write a query to print highest spend month and amount spent in that month for each card type?

```sql
with cte as(
select card_type,datepart(year,transaction_date) as yo 
,datepart(month,transaction_date) as mo 
,sum(amount) as monthly_expense
from credit_card_transactions
group by card_type,datepart(year,transaction_date),datepart(month,transaction_date))

select * from( select *,
rank()over(partition by card_type order by monthly_expense desc) as rn from cte) as a
where rn = 1;
```
Query Explanation:

This SQL query identifies the month with the highest total spending for each card type based on credit card transaction data. It begins by using a Common Table Expression (CTE) named cte, where the transaction data is grouped by card_type, the year, and the month (extracted using the DATEPART function). Within each group, it calculates the total monthly expense using SUM(amount). This results in a summary table showing total spending per card type for each month. In the main query, a ranking function RANK() is applied using a window partitioned by card_type and ordered by monthly_expense in descending order, assigning rank 1 to the month with the highest spending for each card type. This step is performed within a subquery aliased as a. Finally, the outer query filters the data to return only the rows where the rank is 1 (WHERE rn = 1), effectively selecting the month and year of peak spending for each card type. This analysis is useful for identifying seasonal or promotional performance patterns for different card types.


-  ### **`Transaction Details at 1M Cumulative Spend by Card Type`**
3. Write a query to print the transaction details(all columns from the table) for each card type when it reaches a cumulative of 1000000 total spends(We should have 4 rows in the o/p one for each card type)
```sql
# Viewing the Data
select * from(select *,
rank () over(partition by card_type order by cum_sum asc) as rn
 from (
select * , 
sum(amount)  over(partition by card_type order by transaction_date,transaction_id) as cum_sum
from credit_card_transactions) as a
where cum_sum > 1000000)as b
where rn = 1;
```
![3 QUESTION](https://github.com/user-attachments/assets/ac09ec48-d2a4-419c-8130-d78d530afbab)
 
Query Explanation:

This SQL query identifies the first transaction in each card type category where the cumulative spending exceeds 1,000,000 units. It accomplishes this in three key steps: First, it calculates the cumulative sum of transaction amounts for each card_type using the SUM() window function, ordered by transaction_date and transaction_id. This running total (cum_sum) helps in tracking the accumulating spending for each card type chronologically.

Next, the query filters the dataset to only include transactions where the cumulative sum surpasses 1,000,000, isolating the records that exceed this threshold for each card type. Finally, the query ranks these transactions within each card_type using the RANK() window function based on the cumulative sum in ascending order. By setting rn = 1, it ensures that only the first transaction where the cumulative sum exceeds 1,000,000 is selected for each card type.

The result presents the specific transaction details — including transaction ID, date, amount, and cumulative sum — that indicate the precise point where the spending crosses the 1,000,000 mark. This analysis is crucial for financial monitoring, identifying key spending thresholds, and understanding customer behavior across different card types. It can also be leveraged for risk assessment, targeted marketing strategies, and spending pattern analysis.


-  ### **`City with Lowest Gold Card Spend Percentage`**
4.Write a query to find city which had lowest percentage spend for gold card type?

```sql
# Viewing the Data
select city,
sum(amount) as total_expense
,sum(case when card_type = "Gold" then amount else 0 end ) as gold_spent
,sum(case when card_type = "Gold" then amount else 0 end )/sum(amount)*100 as  percentage_contribution
from credit_card_transactions
group by city
having sum(case when card_type = "Gold" then amount else 0 end ) >0
order by percentage_contribution asc	
LIMIT 1;
```

![4 QUESTION](https://github.com/user-attachments/assets/252c02c9-5b2b-4777-8778-36ee1cd27677)


Query Explanation:

This SQL query analyzes credit card transaction data to identify the city with the lowest percentage contribution of "Gold" card transactions to the overall spending. It achieves this in several key steps. First, the query aggregates transaction data at the city level using the GROUP BY clause, calculating the total expense (total_expense) for each city using the SUM(amount) function. Next, it applies a conditional aggregation to separately calculate the total spending by Gold card transactions using a CASE WHEN statement. This isolates the amount spent using Gold cards, storing the result as gold_spent.

The query then computes the percentage contribution of Gold card spending to the overall spending in each city by dividing the Gold card spending by the total expense and multiplying by 100. This calculation is expressed as:
percentage_contribution = gold_spent/total_expense × 100.

​To ensure that only cities with Gold card transactions are considered, the HAVING clause filters the data to retain only those cities where the Gold card spending is greater than zero.
Finally, the query sorts the cities in ascending order of the percentage contribution of Gold card transactions using the ORDER BY percentage_contribution ASC clause. The LIMIT 1 statement restricts the output to a single city — the one with the lowest percentage contribution of Gold card spending to the overall expense.

The result provides insights into identifying the city where the Gold card spending forms the smallest share of the total transactions. This analysis is valuable for identifying underperforming regions for specific card types, allowing targeted promotional strategies to boost spending using Gold cards in those areas.


-  ### **`City, Highest, and Lowest Expense Type`**
5. write a query to print 3 columns:  city, highest_expense_type , lowest_expense_type (example format : Delhi , bills, Fuel)
```sql
with cte as(select city,exp_type,sum(amount) as total_amount
from credit_card_transactions
group by city,exp_type)

,cte2 as(select *,
rank()over(partition by city order by total_amount desc) as highest_rn
,rank()over(partition by city order by total_amount) as lowest_rn
from cte
)

select city,
max(case when highest_rn = 1 then exp_type end )as  highest_expense_type
,min(case when lowest_rn= 1 then exp_type end )as  lowest_expense_type
from cte2
where highest_rn = 1 or lowest_rn = 1
group by city;
```
Note: The image displays only a partial view of the complete results due to screenshot limitations. For the full dataset and comprehensive tables, please execute these queries directly in your SQL environment or refer to the complete result exports available in this repository's data outputs folder.

![5](https://github.com/user-attachments/assets/f75c4782-ed96-47a5-ba4e-e108928c4563)

Query Explanation:

This SQL query identifies, for each city, the expense type with the highest and lowest total spending based on credit card transactions. It begins by using a Common Table Expression (CTE) named cte to aggregate the total amount spent (SUM(amount)) for each combination of city and expense type by grouping the data on city and exp_type. In the next CTE, cte2, it ranks each expense type within a city using the RANK() window function — highest_rn ranks expense types in descending order of spending (so the highest gets rank 1), and lowest_rn ranks them in ascending order (lowest gets rank 1). These rankings allow identification of both the most and least spent-on categories in each city. The final SELECT statement filters rows where either the highest or lowest rank is 1, and then, using conditional aggregation (CASE WHEN inside MAX and MIN), it extracts the names of the expense types with the highest and lowest total spending for each city. The result provides a clear summary of consumer spending behavior across cities, highlighting which expense categories dominate or lag behind in each location.



-  ### **`Female Spend Percentage by Expense Type`**
6. write a query to find percentage contribution of spends by females for each expense type

```sql
select exp_type,sum(amount) as total_spend
,sum(case when gender = 'F' then amount else 0 end) as female_spend
,sum(case when gender = 'F' then amount else 0 end)/sum(amount)*100 as female_Percentage_contribution
from credit_card_transactions
group by exp_type
order by female_Percentage_contribution;
```

![6](https://github.com/user-attachments/assets/969e2c71-c28c-4313-9df6-1456c3427759)

Query Explanation:

This SQL query analyzes credit card transactions to determine the total spending and the female spending contribution for each expense type. It starts by grouping the data by exp_type using the GROUP BY clause to perform aggregations for each category of expense. Within each expense type, it calculates three key metrics: (1) total_spend – the total amount spent using SUM(amount); (2) female_spend – the amount spent by female customers only, using a CASE WHEN condition that includes amount only when the gender is 'F', and assigns 0 otherwise; and (3) female_Percentage_contribution – the percentage of total spending that came from female customers, calculated by dividing female_spend by total_spend and multiplying by 100. The query finally orders the results in ascending order of female_Percentage_contribution to show which expense types had the least to most contribution from female customers, helping to identify consumer behavior trends by gender across different spending categories.

-  ### **`Highest Month-over-Month Growth in Jan-2014`**
  7. which card and expense type combination saw highest month over month growth in Jan-2014




-  ### **`City with Highest Weekend Spend Ratio`**
8. during weekends which city has highest total spend to total no of transcations ratio?
```sql
SELECT city,sum(amount)*1.0/count(*) as ratio
from credit_card_transactions
where datepart(weekday,transaction_date)in(1,7) 
group by city 
order by ratio desc;
```
Query Explanation:

This SQL query analyzes credit card transaction data to calculate the average transaction amount on weekends for each city, and then ranks the cities based on this value. It starts by filtering the data using the WHERE clause to include only transactions that occurred on weekends—specifically on Sunday (1) and Saturday (7), as determined by DATEPART(weekday, transaction_date). Then, using GROUP BY city, the query groups all weekend transactions by city. For each city, it calculates the average transaction amount by dividing the total sum of amount by the total number of transactions (COUNT(*)). To ensure the result is in decimal form (not integer), the total amount is multiplied by 1.0. This average is labeled as ratio. Finally, the cities are sorted in descending order of this ratio using ORDER BY ratio DESC, so the output shows the cities where people spend the most per transaction on weekends at the top. This insight can help identify regions with higher weekend spending behavior.



-  ### **`City with Least Days to Reach 500th Transaction`**
9. which city took least number of days to reach its 500th transaction after the first transaction in that city
```sql
with cte as(select * 
,row_number() over(partition by city order by transaction_date,transaction_id) as rn
from credit_card_transactions)

select city
,min(transaction_date) as first_transaction
,max(transaction_date) as last_transaction
,datediff(day,(transaction_date),max(transaction_date)) as days_to_500 from cte
where rn in (1,500)
group by city 
having count(*) = 2
order by days_to_500 asc;
```

Query Explanation:

This SQL query calculates how many days it took each city to reach its 500th credit card transaction. It begins by using a Common Table Expression (CTE) to assign a unique row number to each transaction within each city using the ROW_NUMBER() function, ordered by transaction date and transaction ID. This effectively ranks all transactions chronologically for every city. The main query then filters this data to retain only the rows where the row number is 1 (the first transaction) or 500 (the 500th transaction). It groups the results by city and uses the HAVING COUNT(*) = 2 clause to ensure only cities that have at least 500 transactions are considered. For each such city, the query calculates the date of the first transaction (MIN(transaction_date)) and the 500th transaction (MAX(transaction_date)), and then uses the DATEDIFF() function to compute the difference in days between them, representing how many days it took the city to reach its 500th transaction. Finally, the results are sorted in ascending order of this duration (days_to_500), showing which cities achieved high transaction volume in the shortest time. This analysis is useful for identifying cities with rapid growth or high engagement in credit card usage.










## ⚙️ SQL Techniques




- Data Filtering
Used WHERE clauses to filter transactions by city, card type, or date range.

- Aggregation
Applied SUM() and COUNT() to calculate total and count of transactions across different dimensions.

- Sorting Results
Used ORDER BY to arrange cities or card types by total spending or transaction volume.

- Date-Based Analysis
Used MONTH() and YEAR() functions to identify monthly and yearly trends in transaction volume and value.

- City-Wise Insights
Grouped data by city to analyze spending behavior and transaction trends across locations.

- Card Type Analysis
Grouped by card_type to understand spending distribution across different cardholders.

- Trend Identification
Combined filtering, grouping, and sorting to identify top-performing cities and card types by expenditure.

- Aliasing
Used AS to rename columns and improve readability of query outputs.

- Ranking and Comparison
Used ROW_NUMBER() and other window functions to rank cities or card types by expenditure.



## 📂 Data Source

Data Set📂:[Credit Card Transactions Dataset](https://www.kaggle.com/datasets/thedevastator/analyzing-credit-card-spending-habits-in-india?resource=download)

- transaction_id: Unique identifier for each credit card transaction.

- city: Indicates the city where the transaction occurred (e.g., Delhi, Greater Mumbai, Bengaluru, Ahmedabad).

- transaction_date: The date on which the transaction was made, formatted as DD-MMM-YY.

- card_type: Specifies the type of credit card used — Gold, Silver, Platinum, or Signature.

- exp_type: Category of expenditure. In this dataset, all transactions fall under the 'Bills' category.

- gender: Gender of the cardholder. This dataset includes only female cardholders (represented as 'F').

- amount: The value of the transaction in Indian Rupees (INR).




## 🛠️ Skills Demonstrated

- SELECT Statements
Used SELECT *, SELECT column_name for basic data retrieval and column-specific queries.

- Filtering with WHERE Clause
Filtered records with conditions like WHERE city = 'Delhi' and WHERE card_type = 'Gold'.

- Sorting with ORDER BY
Sorted data using ORDER BY amount DESC, ORDER BY transaction_date, and more for insights.

- Aggregation Functions
Applied SUM(), COUNT(), AVG() to analyze total spends, transaction counts, and averages by city, card type, or expense type.

- GROUP BY Clause
Grouped data by city, card_type, exp_type, MONTH(transaction_date) for grouped insights.

- JOINs (CROSS JOIN)
Used CROSS JOIN with a CTE to calculate percentage contribution of each city to the total spend.

- Common Table Expressions (CTEs)
Created modular and readable queries using WITH clause to calculate ranks, expenses, and growth across partitions.

- Window Functions
Implemented ROW_NUMBER() and RANK() with PARTITION BY and ORDER BY to find cumulative spending points, highest/lowest ranks, and top-performing groups.

- Aliases (AS)
Used column aliases like AS monthly_expense, AS percentage_contribution, AS cum_sum for readability.

- Date Functions
Used MONTH(), YEAR(), DATEDIFF(), DATEPART() to analyze monthly trends, transaction gaps, and weekdays/weekends.

- Conditional Aggregation (CASE WHEN)
Used CASE WHEN inside aggregation functions to compute conditional metrics like gender-based spends or expense-type contributions.

- Data Cleaning with ALTER TABLE
Renamed and reformatted columns (RENAME COLUMN, MODIFY COLUMN) to standardize data structure.

- Subqueries and Nested Queries
Structured multi-step logic using subqueries to determine top cities, cumulative spending milestones, and transaction milestones.

- LIMIT Clause
Restricted output to top entries using LIMIT 1, LIMIT 5 to identify key records such as top cities or top transactions.

- INSERT Statements
Performed INSERT INTO to test custom records for validation and debugging purposes.

