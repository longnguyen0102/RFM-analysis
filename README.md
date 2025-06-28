# RFM analysis - Retail - Python

Please check the coding below or access via the link below:  
🔗 https://colab.research.google.com/drive/1cWVm4ttAOOfBJzVmswV8OovZ3s1xOa8H?usp=sharing 🔗    

Author: Nguyễn Hải Long  
Date: 2025-05  
Tools Used: Python  

---

## 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)  
3. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview  

### Objective:
### 📖 This project is about using Python to analyze given dataset.

✔️ 

### 👤 Who is this project for?  

✔️ Data analysts & business analysts.  
✔️ Decision-makers.

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  
- Source: 
- Size: The dataset has 3 tables: payment_report (5 columns, 919 rows), product (3 columns, 492 rows), and transactions (9 columns, 1324002 rows).
- Format: .csv

### 📊 Data Structure & Relationships  
#### 1️⃣ Table used: 
Using all 3 tables of the dataset.  

#### 2️⃣ Table Schema & Data Snapshot:  
Table 1: payment_report  

| Field Name | Data Type |
|------------|-----------|
| report_month | object |
| payment_group | object |
| product_id | object |
| source_id | int64 |
| volume | int64 |

Table 2: product  

| Field Name | Data Type |
|------------|-----------|
| product_id | int64 |
| category | object |
| team_own | object |

Table 3: transactions

| Field Name | Data Type |
|------------|-----------|
| transaction_id | int64 |
| merchant_id | int64 |
| volume | int64 |
| transType | int64 |
| transStatus | int64 |
| sender_id | float64 |
| receiver_id | float64 |
| extra_info | object |
| timeStamp | int64 |

---

## ⚒️ Main Process

### 1️⃣ EDA
#### Import libraries and dataset, copy dataset:
```
# import libraries
import pandas as pd
import numpy as np
from google.colab import drive

# import csv files
from google.colab import drive
drive.mount('/content/drive')

path_payment_report = '/content/drive/MyDrive/DAC K34/Python/Project_2/payment_report.csv'
path_product = '/content/drive/MyDrive/DAC K34/Python/Project_2/product.csv'
path_transactions = '/content/drive/MyDrive/DAC K34/Python/Project_2/transactions.csv'

payment_report = pd.read_csv(path_payment_report)
product = pd.read_csv(path_product)
transactions = pd.read_csv(path_transactions)

# copy dataset
df_payment_report = payment_report
df_product = product
df_transactions = transactions
```

#### Dataframe df_payment_report:  
Understand about data type / data value
```
df_payment_report.head()

# show rows and columns count
print(f'Rows count: {df_payment_report.shape[0]}\nColums count: {df_payment_report.shape[1]}')

# show data type
df_payment_report.info()

# further checking on columns
df_payment_report.shape
df_payment_report.describe()
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_df_payment_report_eda_1.png)  

Checking unique & missing values
```
# check null values
df_payment_report.isnull().sum()

# check unique values
## print the percentage of unique
num_unique = df_payment_report.nunique().sort_values()
print('---Percentage of unique values (%)---')
print(100/num_unique)

# check missing data
missing_value = df_payment_report.isnull().sum().sort_values(ascending = False)
missing_percent = df_payment_report.isnull().mean().sort_values(ascending = False)
print('')
print('---Number of missing values in each column---')
print(missing_value)
print('')
print('---Percentage of missing values (%)---')
if missing_percent.sum():
  print(missing_percent[missing_percent > 0] * 100)
else:
  print('None')

# check for duplicates
## show number of duplicated rows
print('')
print(f'Number of entirely duplicated rows: {df_payment_report.duplicated().sum()}')
## show all duplicated rows
df_payment_report[df_payment_report.duplicated()]
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_df_payment_report_eda_2.png)  

Handle missing values: There are no missing values.  

Handle duplicated values: There are no duplicated values.  

#### Dataframe df_product:  
Understand about data type / data value  
```
df_product.head()

# show rows and columns count
print(f'Rows count: {df_product.shape[0]}\nColums count: {df_product.shape[1]}')

# show data type
df_product.info()

# further checking on columns
df_product.shape
df_product.describe()
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_df_product_eda_1.png)

Checking unique & missing values
```
# check null values
df_product.isnull().sum()

# check unique values
## print the percentage of unique
num_unique = df_product.nunique().sort_values()
print('---Percentage of unique values (%)---')
print(100/num_unique)

# check missing data
missing_value = df_product.isnull().sum().sort_values(ascending = False)
missing_percent = df_product.isnull().mean().sort_values(ascending = False)
print('')
print('---Number of missing values in each column---')
print(missing_value)
print('')
print('---Percentage of missing values (%)---')
if missing_percent.sum():
  print(missing_percent[missing_percent > 0] * 100)
else:
  print('None')

# check for duplicates
## show number of duplicated rows
print('')
print(f'Number of entirely duplicated rows: {df_product.duplicated().sum()}')
## show all duplicated rows
df_product[df_product.duplicated()]
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_df_product_eda_2.png)  

Handle missing values: There are no missing values.  

Handle duplicated values: There are no duplicated values.  

#### Dataframe df_transactions
Understand about data type / data value 
```
df_transactions.head()

# show rows and columns count
print(f'Rows count: {df_transactions.shape[0]}\nColums count: {df_transactions.shape[1]}')

# show data type
df_transactions.info()

# further checking on columns
df_transactions.shape
df_transactions.describe()
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_df_transactions_eda_1.png)  

Checking unique & missing values  
```
# check null values
df_transactions.isnull().sum()

# check unique values
## print the percentage of unique
num_unique = df_transactions.nunique().sort_values()
print('---Percentage of unique values (%)---')
print(100/num_unique)

# check missing data
missing_value = df_transactions.isnull().sum().sort_values(ascending = False)
missing_percent = df_transactions.isnull().mean().sort_values(ascending = False)
print('')
print('---Number of missing values in each column---')
print(missing_value)
print('')
print('---Percentage of missing values (%)---')
if missing_percent.sum():
  print(missing_percent[missing_percent > 0] * 100)
else:
  print('None')

# check for duplicates
## show number of duplicated rows
print('')
print(f'Number of entirely duplicated rows: {df_transactions.duplicated().sum()}')
## show all duplicated rows
df_transactions[df_transactions.duplicated()]
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_df_transactions_eda_2.png)

Handle incorrect data type: 
 - timeStamp -> change to datetime data type.
 - sender_id -> change to int64 data type.
 - receiver_id -> change to int64 data type.

Handle missing data:  
- sender_id -> need to validate data with data provider/ fill up data according to extra_info (if available).
- receiver_id -> need to validate data with data provider/ fill up data according to extra_info (if available).  
- extra_info -> need to validate data with data provider/ fill up data according to sender_id (if available).  

Handle duplicated values: As can see from the table above, there are 28 rows with duplicated data. They are from "extra_info" column and they are missing values. In this case, we will use method from "Handle missing values" part and no need to drop these rows.     

#### Create df payment_enriched (merge payment_report.csv with product.csv)
```
payment_enriched = df_payment_report.merge(df_product, on='product_id', how='left')
payment_enriched.info()
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_create_df_payment_enriched.png)  

### 2️⃣ Data wrangling

#### 1/ Top 3 product_ids with the highest volume.  
```
df_top_3 = df_payment_report.groupby('product_id')['volume'].sum().reset_index()

df_top_3 = df_top_3.sort_values(by=['volume'], ascending=False)

df_top_3.head(3)
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_query_1_result.png)  

#### 2/ Given that 1 product_id is only owed by 1 team, are there any abnormal products against this rule?  
```
df_1_product_1_team = payment_enriched.groupby('product_id')['team_own'].nunique().reset_index()

df_1_product_1_team[df_1_product_1_team['team_own'] != 1]
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_query_2_result.png)  

#### 3/ Find the team has had the lowest performance (lowest volume) since Q2.2023. Find the category that contributes the least to that team.  
```
df_low = payment_enriched[payment_enriched['report_month'] >= '2023-04']

df_low
```
Result: 
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_query_3_result.png)  

#### 4/ Find the contribution of source_ids of refund transactions (payment_group = ‘refund’), what is the source_id with the highest contribution?  
```
payment_report_refund = df_payment_report[payment_report.payment_group == 'refund']
payment_report_refund = payment_report_refund[['source_id','volume']].groupby(by=['source_id']).sum().reset_index().sort_values('volume', ascending=False)

payment_report_refund.head()
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_query_4_result.png)

#### 5/ Define type of transactions (‘transaction_type’) for each row, given:  
#### - transType = 2 & merchant_id = 1205: Bank Transfer Transaction  
#### - transType = 2 & merchant_id = 2260: Withdraw Money Transaction  
#### - transType = 2 & merchant_id = 2270: Top Up Money Transaction  
#### - transType = 2 & others merchant_id: Payment Transaction  
#### - transType = 8, merchant_id = 2250: Transfer Money Transaction  
#### - transType = 8 & others merchant_id: Split Bill Transaction  
#### - Remained cases are invalid transactions  
```
def trans_type(row):
  # condition Bank Transfer tra cứu với transType và merchant_id
  ## run each row of transType và merchant_id
  transType = row['transType']
  merchant_id = row['merchant_id']

  # split into 3 cases: transType = 2, transType = 8 and else.
  if transType == 2:
    if merchant_id == 1205:
      return 'Bank Transfer Transaction'
    elif merchant_id == 2260:
      return 'Withdraw Money Transaction'
    elif merchant_id == 2270:
      return 'Top Up Money Transaction'
    else:
      return 'Payment Transaction'
  elif transType == 8:
    if merchant_id == 2250:
      return 'Transfer Money Transaction'
    else:
      return 'Split Bill Transacion'
  else:
    return 'Invalid Transaction'

# apply trans_type
df_transactions['transaction_type']=df_transactions.apply(trans_type, axis=1)
df_transactions.head()
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_query_5_result.png)

#### 6/ Of each transaction type (excluding invalid transactions): find the number of transactions, volume, senders and receivers.
```
valid_transactions = df_transactions[df_transactions['transaction_type'] != 'Invalid Transaction']

valid_transactions
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_query_6_result_1.png)  

```
valid_transactions.groupby(by = 'transaction_type').agg(total_transactions=('transaction_id', 'nunique'),
                                                        total_volume=('volume', 'sum'),
                                                        sender_count=('sender_id', 'nunique'),
                                                        receiver_count=('receiver_id', 'nunique'))
```
Result:  
![](https://github.com/longnguyen0102/photo/blob/main/data_wrangling-fintech-python/python_data_wrangling_query_6_result_2.png)

## 📌 Key Takeaways:  
✔️ Understanding the basics of SQL query.  
✔️ Know how to apply Window Functions when writing queries.  
✔️ Understanding real-world requirements when using SQl to retrieve neceesary data
