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

✔️ SuperStore is a global retail company. To celebrate Christmas and New Year, Marketing team wants to deploy marketing campaigns in order to show appreciation to loyalty customers. Beside that, they want to engage with potential customers who could become loyal clients.  
✔️ Marketing director suggests using RFM model in Python to classify customers, then launch marketing campaigns to appreciate loyalty customers, as well as engaging potential customers.  

### 👤 Who is this project for?  

✔️ Data leaders.  
✔️ Marketing team leaders.  
✔️ Sales team leaders.  

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  
- Source: Company database.  
- Size: The dataset is 01 excel file with 2 sheets: 'ecommerce retail' & 'Segmentation'.  
- Format: .xlsx

### 📊 Data Structure & Relationships  
#### 1️⃣ Table used: 
Using the whole dataset.  

#### 2️⃣ Table Schema & Data Snapshot:  
<details>
 <summary>Table using in this project:</summary>
  
| Field Name | Data Type | Description |
|------------|-----------|-------------|
| InvoiceNo | object | Invoice number. Nominal, a 6-digit integral number uniquely assigned to each transaction. If this code starts with letter 'C', it indicates a cancellation. |
| StockCode | object | Product (item) code. Nominal, a 5-digit integral number uniquely assigned to each distinct product. |
| Description | object | Product (item) name. Nominal. |
| Quantity | int64 | The quantities of each product (item) per transaction. Numeric. |
| InvoiceDate | datetime64 | Invoice Date and time. Numeric, the day and time when each transaction was generated. |
| UnitPrice | float64 | Unit price. Numeric, Product price per unit in sterling. |
| CustomerID | float64 | Customer number. Nominal, a 5-digit integral number uniquely assigned to each customer. |
| Country | object | Country name. Nominal, the name of the country where each customer resides. |

</details>
---

## ⚒️ Main Process

### 1️⃣ EDA
*Note: Click the arrow to see codes*  
#### Import libraries and dataset, copy dataset:
<details>
 <summary>Code:</summary>
  
```
# import libraries
import pandas as pd
import numpy as np
from google.colab import drive
import matplotlib.pyplot as plt
import seaborn as sns

# import excel files with sheet name 'ecommerce retail'
drive.mount('/content/drive')

path = '/content/drive/MyDrive/DAC K34/Python/Project_3/ecommerce retail.xlsx'
ecommerce_retail = pd.read_excel (path, sheet_name ='ecommerce retail')

#copy dataframe
df = ecommerce_retail.copy()
```
</details>  

#### Understanding data    
<details>
 <summary>Basic data exploration:</summary>

```
df.head()

# show rows and columns count
print(f'Rows count: {df.shape[0]}\nColums count: {df.shape[1]}')

# show data type
df.info()

# further checking on columns
df.shape
df.describe()

# check null values
df.isnull().sum()

# check unique values
## print the percentage of unique
num_unique = df.nunique().sort_values()
print('')
print('---Percentage of unique values (%)---')
print(100/num_unique)

# check missing data
missing_value = df.isnull().sum().sort_values(ascending = False)
missing_percent = df.isnull().mean().sort_values(ascending = False)
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
print(f'Number of entirely duplicated rows: {df.duplicated().sum()}')
## show all duplicated rows
df[df.duplicated()]
```

 </details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_1.png)  
![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_2.png)

<details>
 <summary>Change data typpe of 'InvoiceNo' to string:</summary>

```
# change data type of Invoice No to string
df['InvoiceNo'] = df['InvoiceNo'].astype(str)
```

</details>

<details>
 <summary>Explore negative values of Quantity columns (Quantity < 0):</summary>
  
```
# print out some rows where Quantity < 0
print('Some rows have Quantity < 0')
print(df[df['Quantity']<0].head())


# further checking
## make a new column: True if InvoiceNo has 'C', False if InvoiceNo has no 'C'
df['Cancellation'] = df['InvoiceNo'].str.contains('C')

## check InvoiceNo has 'C' and Quantity < 0
print(df[(df['Cancellation'] == True) & (df['Quantity'] < 0)].head())
print('asoidfbao',df['CustomerID'].isna().sum())

## check InvoiceNo has no 'C' and Quantity < 0
print(df[(df['Cancellation'] == False) & (df['Quantity'] < 0)].head())
```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_3.png)

<details>
 <summary>Explore negative values of Quantity columns (UnitPrice < 0):</summary>
  
```
# print out some rows where Quantity < 0
print('Some rows have UnitPrice < 0')
print(df[df['UnitPrice'] < 0].head())
```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_4.png)

<details>
 <summary>Seperate 'InvoiceData' to 'Day' and 'Month' columns:</summary>
  
```
# seperate InvoiceDate to Day and Month columns
df['Day'] = pd.to_datetime(df.InvoiceDate).dt.date
df['Month'] = df['Day'].apply(lambda x: str(x)[:-3])
df.head()
```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_5.png)

#### Handle negative, missing values, duplicates:  

<details>
 <summary>Negative values:</summary>
  
```
# change data type
df['StockCode'] = df['StockCode'].astype(str)
df['Description'] = df['Description'].astype(str)
df['CustomerID'] = df['CustomerID'].astype(str)
df['Country'] = df['Country'].astype(str)

# drop negative values in Quantity and UnitPrice column
df = df[df['Quantity'] > 0]
df = df[df['UnitPrice'] > 0]

# drop InvoiceNo with C
df = df[df['Cancellation'] == False]

# replace NaN
df = df.replace('nan', None)
df = df.replace('Nan', None)

df.info()
```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_6.png)

<details>
 <summary>Missing values:</summary>
  
```
# show up some rows with missing values
print('---Some rows with missing values---')
df_null = df.isnull()
rows_with_null = df_null.any(axis=1)
df_with_null = df[rows_with_null]
print(df_with_null.head(10))
```
![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_7.png)

```
# drop rows with CustomerID == None
df_no_na = df.drop(df[df['CustomerID'].isnull()].index)
df_no_na
```
</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_8.png)

<details>
 <summary>Duplicated values:</summary>
  
```
# locate the values are not duplicated in the selected columns
df_no_dup = df_no_na.loc[~df.duplicated(subset = ['InvoiceNo','StockCode','InvoiceDate','UnitPrice','CustomerID','Country'])].reset_index(drop=True).copy()

# check an example of duplicate in InvoiceNo
df_no_dup.query('InvoiceNo == "536365"')

df_no_dup.query('InvoiceNo == "581587"')
```

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_9.png)

```
# drop duplicates, keep the first row of subset
df_main = df.drop_duplicates(subset=["InvoiceNo", "StockCode","InvoiceDate","CustomerID"], keep = 'first')

df_main.head()
```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_10.png)

<details>
 <summary>Create 'Sales' column (Quantity * Price):</summary>
  
```
# create Sales column (Quantity * UnitPrice)
df_main['Sales'] = df_main.Quantity * df.UnitPrice

# take max('Day') for recently interaction of customer
last_day = df_main.Day.max()

last_day
df_main
```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_11.png)

## 📌 Key Takeaways:  
✔️ Understanding the basics of SQL query.  
✔️ Know how to apply Window Functions when writing queries.  
✔️ Understanding real-world requirements when using SQl to retrieve neceesary data
