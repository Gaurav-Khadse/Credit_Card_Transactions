# Credit_Card_Transactions


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


Data Set📂:[Spotify Dataset](https://www.kaggle.com/datasets/thedevastator/analyzing-credit-card-spending-habits-in-india?resource=download)

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
select * from transaction_id;
select * from city;
select * from transaction_date;
select * from card_type;
select * from exp_type;
select * from gender;
select * from amount;

```




