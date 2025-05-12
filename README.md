# Credit_Card_Transactions


## 📑 Contents

- [**🌐 Introduction**](#-introduction)
- [**💾 Data Setup in SQL Workbench**](#-data-setup-in-sql-workbench)
- [**🔍 Exploring the Dataset**](#-exploring-the-dataset)
- [**🔍 Analytical Insights**](#-analytical-insights)
- [**⚙️ SQL Techniques**](#️-sql-techniques)
- [**📂 Data Source**](#-data-source)
- [**🛠️ Skills Demonstrated**](#️-skills-demonstrated)



![image](https://github.com/user-attachments/assets/32b2b489-923a-44a6-8843-290c1648561d)



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

-  ### **`Female Spend Percentage by Expense Type`**
6. write a query to find percentage contribution of spends by females for each expense type
-  ### **`Highest Month-over-Month Growth in Jan-2014`**
  

7. which card and expense type combination saw highest month over month growth in Jan-2014
-  ### **`City with Highest Weekend Spend Ratio`**
  
8. during weekends which city has highest total spend to total no of transcations ratio  
-  ### **`City with Least Days to Reach 500th Transaction`**
9. which city took least number of days to reach its 500th transaction after the first transaction in that city

