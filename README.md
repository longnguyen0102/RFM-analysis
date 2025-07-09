# RFM analysis a retailing company - SuperStore using Python

Please check the code below or access via the link:  
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

✔️ SuperStore is a global retail company. **To celebrate Christmas and New Year**, Marketing team wants to deploy **marketing campaigns** in order to show appreciation to loyalty customers. Beside that, they want to engage with potential customers who could become loyal clients.  
✔️ Marketing director suggests using RFM model in Python to classify customers, then launch marketing campaigns to appreciate loyalty customers, as well as engaging potential customers.  
✔️ RFM analysis (Recency - Frequency - Monetary) is a marketing technique used to quantitatively rank and group customers based on the recency, frequency and monetary total of their recent transactions to identify the best customers and perform targeted marketing campaigns.  

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

Sheet 'ecommercer  retail'  

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

Sheet 'Segmentation'

| Segment | RFM Score |
|---------|-----------|
| Champions | 555, 554, 544, 545, 454, 455, 445 |
| Loyal	| 543, 444, 435, 355, 354, 345, 344, 335 |
| Potential Loyalist	| 553, 551, 552, 541, 542, 533, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323 |
| New Customers	| 512, 511, 422, 421, 412, 411, 311 |
| Promising	| 525, 524, 523, 522, 521, 515, 514, 513, 425,424, 413,414,415, 315, 314, 313 |
| Need Attention	| 535, 534, 443, 434, 343, 334, 325, 324 |
| About To Sleep	| 331, 321, 312, 221, 213, 231, 241, 251 |
| At Risk	| 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124 |
| Cannot Lose Them	| 155, 154, 144, 214,215,115, 114, 113 |
| Hibernating customers	| 332, 322, 233, 232, 223, 222, 132, 123, 122, 212, 211 |
| Lost customers	| 111, 112, 121, 131,141,151 |

</details>

- Sheet 'ecommerce retail' will provide data for EDA, calculating.  
- Using sheet 'Segmentation' for segment customers based on the score.

---

## ⚒️ Main Process  

*Note: Click the white triangle to see codes*  

### 1️⃣ EDA
<details>
 <summary>Import libraries and dataset, copy dataset:</summary>
  
  ```python
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

 ```python
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

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_1.png)  
![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_2.png)

</details>

➡️ This dataset has 8 columns and 541,909 records. Most columns have right data type:  
- InvoiceNo (object) -> no need to change data type.  
- StockCode (object) -> no need to change data type.  
- Description (object) -> no need to change data type.  
- Quantity (int64) -> no need to change data type.  
- InvoiceDate (datetime) -> no need to change data type.  
- UnitPrice (float64) -> no need to change data type.  
- CustomerID (float64) -> can be changed to int64 if needed.
- Country (object) -> no need to change data type.  
➡️ 

<details>
 <summary>Change data typpe of 'InvoiceNo' to string:</summary>

 ```python
 # change data type of Invoice No to string
 df['InvoiceNo'] = df['InvoiceNo'].astype(str)
 ```

</details>

<details>
 <summary>Explore negative values of Quantity columns (Quantity < 0):</summary>
  
 ```python
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
  
 ```python
 # print out some rows where Quantity < 0
 print('Some rows have UnitPrice < 0')
 print(df[df['UnitPrice'] < 0].head())
 ```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_4.png)

<details>
 <summary>Seperate 'InvoiceData' to 'Day' and 'Month' columns:</summary>
  
 ```python
 # seperate InvoiceDate to Day and Month columns
 df['Day'] = pd.to_datetime(df.InvoiceDate).dt.date
 df['Month'] = df['Day'].apply(lambda x: str(x)[:-3])
 df.head()
 ```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_5.png)

➡️ The 'InvoiceDate' column is split into 'Day' and 'Month' to later identify the customer's most recent interaction date, which is essential for calculating the Recency metric.  

#### Handle negative, missing values, duplicates:  

<details>
 <summary>Negative values:</summary>
  
 ```python
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

➡️ Remove all negative values in 'Quantity', 'UnitPrice' and 'InvoiceNo' with 'C' because they are cancelled orders.  

<details>
 <summary>Missing values:</summary>
  
 ```python
 # show up some rows with missing values
 print('---Some rows with missing values---')
 df_null = df.isnull()
 rows_with_null = df_null.any(axis=1)
 df_with_null = df[rows_with_null]
 print(df_with_null.head(10))
 ```
 ![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_7.png)
 
 ```python
 # drop rows with CustomerID == None
 df_no_na = df.drop(df[df['CustomerID'].isnull()].index)
 df_no_na
 ```
</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_8.png)

➡️ Drop all rows with 'CustomerID' is null. The reason for this action is cannot identify the customers.  

<details>
 <summary>Duplicated values:</summary>
  
 ```python
 # locate the values are not duplicated in the selected columns
 df_no_dup = df_no_na.loc[~df.duplicated(subset = ['InvoiceNo','StockCode','InvoiceDate','UnitPrice','CustomerID','Country'])].reset_index(drop=True).copy()
 
 # check an example of duplicate in InvoiceNo
 df_no_dup.query('InvoiceNo == "536365"')
 
 df_no_dup.query('InvoiceNo == "581587"')
 ```
 
 ![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_9.png)
 
 ```python
 # drop duplicates, keep the first row of subset
 df_main = df.drop_duplicates(subset=["InvoiceNo", "StockCode","InvoiceDate","CustomerID"], keep = 'first')
 
 df_main.head()
 ```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_10.png)

<details>
 <summary>Create 'Sales' column (Quantity * Price):</summary>
  
 ```python
 # create Sales column (Quantity * UnitPrice)
 df_main['Sales'] = df_main.Quantity * df.UnitPrice
 
 # take max('Day') for recently interaction of customer
 last_day = df_main.Day.max()
 
 last_day
 df_main
 ```

</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_eda_11.png)

➡️ Taking max of 'Day' in order to identify the most recent date of interaction of customers.  

### 2️⃣ Data processing  

#### Handle Segmentation table  

<details>
 <summary>Code:</summary>  

 ```python
 # import excel files with sheet name 'Segmentation'
 segmentation = pd.read_excel (path, sheet_name ='Segmentation')
 
 # copy dataframe
 df_seg = segmentation
 
 # transform Segmentation
 df_seg['RFM Score'] = df_seg['RFM Score'].str.split(',')
 df_seg = df_seg.explode('RFM Score').reset_index(drop=True)
 
 df_seg.head()
 ```

</details>

![data_processing_1](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_data_processing_1.png)  

➡️ The Segmentation copy process involves duplicating a new Segmentation table to avoid interference with the original dataset, thereby preventing unintended data modifications. The transformation of the Segmentation table will split segments based on predefined RFM scores. These scores are currently separated by commas, so this process will parse them into the required segments accordingly.  

#### Calculating RFM

<details>
 <summary>Code:</summary>
 
 ```python
 # determining Recency, Frequency, Monetary
 df_RFM = df_main.groupby('CustomerID').agg(
     Recency = ('Day', lambda x: last_day - x.max()),
     Frequency = ('CustomerID','count'),
     Monetary = ('Sales','sum'),
     Start_Date = ('Day','min')
 ).reset_index()
 
 df_RFM['Recency'] = df_RFM['Recency'].dt.days.astype('int16')
 # take opposite of Recency
 df_RFM['Reverse_Recency'] = -df_RFM['Recency']
 df_RFM['Start_Date'] = pd.to_datetime(df_RFM.Start_Date)
 
 # label R, F, M
 df_RFM['R'] = pd.qcut(df_RFM['Reverse_Recency'], 5, labels = range(1,6)).astype(str)
 df_RFM['F'] = pd.qcut(df_RFM['Frequency'], 5, labels = range(1,6)).astype(str)
 df_RFM['M'] = pd.qcut(df_RFM['Monetary'], 5, labels = range(1,6)).astype(str)
 df_RFM['RFM'] = df_RFM.R + df_RFM.F + df_RFM.M
 
 df_RFM.head()
 ```
 ![data_processing_2](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_data_processing_2.png)

 ```python
 # clear space
 df_seg['RFM Score'] = df_seg['RFM Score'].str.strip()
 
 # merge with Segementation for comparison
 df_RFM_final = df_RFM.merge(df_seg, how='left', left_on='RFM', right_on='RFM Score')
 
 df_RFM_final.head()
 ```

</details>

![data_processing_3](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_data_processing_3.png)

➡️ In this stage, RFM is calculated:  
- Recency is computed as the last purchase date minus the dataset’s maximum date, which returns a negative value representing the number of days since the most recent transaction.  
- Frequency measures how often a customer makes a purchase and is computed as counting the number of appearance of each customer.  
- Monetary represents the total of money spending from each customer.
Afterward, the results of the three metrics are assigned scores on a scale from 1 to 5.
In the final step, the combined RFM scores are matched against the Segmentation table to assign each customer to a corresponding segment.  

#### Determine Loyal and Non Loyal and showing characteristic of Potential Loyalist  

<details>
 <summary>Loyal status:</summary>

 ```python
 df_RFM_final['Loyal_Status'] = df_RFM_final['Segment'].apply(lambda x: 'Loyal' if x in ('Loyal','Potential Loyalist') else 'Non Loyal')

 df_RFM_final.head()
 ```
</details>

![data_processing_4](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_data_processing_4.png)

<details>
 <summary>Creating df_RFM_final for visualization:</summary>
 
 ```python
 # Average of Quantity and Sales according to CustomerID
 df_potential_average = df_main.groupby('CustomerID').agg(
     Quantity_Average = ('Quantity','mean'),
     Sales_Average = ('Sales','mean')
 ).reset_index()
 
 df_potential_average.head()
 ```

![data_processing_5](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_data_processing_5.png)

 ```python
 # First Sales and Quantity according to CustomerID
 ## base on InvoiceDate to get first order -> Quantity, Sales
 df_main['Ranking'] = df_main.groupby('CustomerID')['InvoiceDate'].rank(method = 'first')
 df_potential_first = df_main[df_main.Ranking == 1][['CustomerID','Quantity','Sales']]
 df_potential_first = df_potential_first.rename(columns={'Quantity':'First_Quantity','Sales':'First_Sales'})
 
 df_potential_first.head()
 ```
 
 ![data_processing_6](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_data_processing_6.png)

 ```python
 # merge all
 df_RFM_final = df_RFM_final.merge(df_potential_average, how = 'left', on = 'CustomerID')
 df_RFM_final = df_RFM_final.merge(df_potential_first, how = 'left', on = 'CustomerID')
 
 df_RFM_final.head()
 ```
</details>

|  | CustomerID | Recency | Frequency | Monetary | Start_Date | Reverse_Recency | R | F | M | RFM | Segment | RFM Score | Loyal_Status | Quantity_Average | Sales_Average | First_Quantity | First_Sales |
|---|-----------|---------|-----------|----------|------------|-----------------|---|---|---|-----|---------|-----------|--------------|------------------|---------------|----------------|--------|
| 0 | 12346.0 | 325 | 1 | 77183.60 | 2011-01-18 | -325 | 1 | 1 | 5 | 115 | Cannot Lose Them | 115 | Non Loyal | 74125.000000 | 77183.000000 | 74215 | 77183.6 |
| 1 | 12347.0 | 2 | 182 | 4310.00 | 2010-12-07 | -2 | 5 | 5 | 5 | 555 | Champions | 555 | Non Loyal | 13.505495 | 23.681319 | 12 | 25.2 |
| 2 | 12348.0 | 75 | 27 | 1595.64 | 2010-12-16 | -75 | 2 | 2 | 4 | 224 | At Risk | 224 | Non Loyal | 68.925926 | 59.097778 | 72 | 39.6 |
| 3 | 12349.0 | 18 | 73 | 1757.55 | 2011-11-21 | -18 | 4 | 4 | 4 | 444 | Loyal | 444 | Loyal | 8.643836 | 24.076027 | 2 | 15.0 |
| 4 | 12350.0 | 310 | 17 | 334.40 | 2011-02-02 | -310 | 1 | 2 | 2 | 122 | Hibernating customers | 122 | Non Loyal | 11.588235 | 19.670588 | 12 | 25.2 |

### 3️⃣ Visualization  

<details>
 <summary>Visualize final dataset with RFM:</summary>

 ```python
 # Visualize spending amount and number of user according to Segment.
 user_by_segment = df_RFM_final[['Segment','CustomerID']].groupby(['Segment']).count().reset_index()
 user_by_segment = user_by_segment.rename(columns = {'CustomerID':'user_volume'})
 user_by_segment['contribution_percent'] = round(user_by_segment['user_volume'] / user_by_segment['user_volume'].sum() * 100)
 user_by_segment['type'] = 'user contribution'
 
 spending_by_segment = df_RFM_final[['Segment','Monetary']].groupby(['Segment']).sum().reset_index()
 spending_by_segment = spending_by_segment.rename(columns = {'Monetary':'spending'})
 spending_by_segment['contribution_percent'] = spending_by_segment['spending'] / spending_by_segment['spending'].sum() * 100
 spending_by_segment['type'] = 'spending contribution'
 
 segment_agg = pd.concat([user_by_segment, spending_by_segment])
 
 plt.figure(figsize=(20, 10))
 sns.barplot(segment_agg, x='Segment', y='contribution_percent', hue='type')
 plt.title='Spending amount and number of user according to Segment'
 
 plt.show()
 ```
</details>

![](https://github.com/longnguyen0102/photo/blob/main/RFM_analysis-retail-python/RFM_analysis-retail-python_visualization.png)

## 📌 Key Takeaways:  
✔️ As showing in the plot, **Champions** customers spend the most and they contribute almost 20%. However, we should mind about **At Risk** customers and **Hibernating** customers as they have 10-20%  
➡️ Action: We should deploy some program to encourage them and notify them about new programs.  

✔️ **Potential Loyalist** customers and **Loyal** customers have user contribution at the same but the money spending of **Potential Loyalist** is low (less than 10%). **Promising** customers are low on user contribution and the money spending is the same as **Potential**  
➡️ Action: Deploy encouraging programs.
