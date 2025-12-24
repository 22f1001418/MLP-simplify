# Week 1: Introduction to Machine Learning

## 🎯 Learning Objectives

- Understand what Machine Learning is
- Differentiate ML from traditional programming
- Get familiar with basic terminology


## 🎥 Video Explanation

> 📌 **Lecture Recording:** _[Add your video link here]_


## 🧠 Conceptual Overview

```python
import pandas as pd
import numpy as np
```

# Pandas Exercises

## Exercise 1: Sales Data Analysis





```python
np.random.seed(42)
dates = pd.date_range('2023-01-01', '2023-03-31')
products = ['Laptop', 'Phone', 'Tablet', 'Monitor', 'Keyboard']
regions = ['North', 'South', 'East', 'West']

sales_data = pd.DataFrame({
    'Date': np.random.choice(dates, 500),
    'Product': np.random.choice(products, 500),
    'Region': np.random.choice(regions, 500),
    'Units_Sold': np.random.randint(1, 20, 500),
    'Unit_Price': np.random.uniform(100, 2000, 500).round(2),
    'Customer_ID': np.random.randint(1000, 1100, 500)
})
sales_data['Total_Sales'] = sales_data['Units_Sold'] * sales_data['Unit_Price']
```

```python
sales_data
```

```python
sales_data.head()
```

```python
sales_data.describe()
```

```python
sales_data.info()
```

```python
sales_data.isna().sum().sort_values(ascending=False)
```

```python
sales_data.shape
```

### Tasks:

1. **Basic Indexing**:
   - Select all sales from the 'North' region
   - Select sales of 'Laptop' or 'Phone' with more than 10 units sold
   - Select columns 'Product', 'Region', and 'Total_Sales' for all records

2. **Grouping Operations**:
   - Find the total sales by product
   - Calculate the average units sold by region
   - Find the top 3 customers by total spending

3. **Filtering**:
   - Find all sales above $5000
   - Find the most expensive single sale for each product
   - Find all sales that occurred on weekends

4. **Apply Functions**:
   - Create a new column 'Price_Category' that labels each sale as:
     - 'Budget' if Unit_Price < 300
     - 'Mid-Range' if 300 ≤ Unit_Price < 1000
     - 'Premium' if Unit_Price ≥ 1000

```python
# Solution

# 1.1
sales_data['Region'].unique()
```

### 1.1

```python
sales_data.Region.value_counts()
```

```python
sales_data[sales_data['Region']=='North']
```

```python
sales_data.Product.unique()
```

```python
sales_data[(sales_data.Product.isin(['Laptop', 'Phone'])) & (sales_data.Units_Sold > 10)]
```

```python
?sales_data.Product.isin
```

### 1.2

```python
sales_data
```

### 1.3



```python
sales_data.loc[:, ['Product', 'Region', 'Total_Sales']]
```

```python
sales_data.iloc[:, [1,2,6]]
```

```python
l = sales_data.columns.to_list()
l.index('Date')
```

```python
sales_data.columns.get_loc('Region')
```

```python
sales_data[['Product', 'Region', 'Total_Sales']]
```

### 2.1 Find the total sales by product

```python
sales_data
```

```python
sales_data.groupby('Product')['Total_Sales'].sum()
```

### 2.3 Find the top 3 customers by total spending

```python
groupedData = sales_data.groupby('Customer_ID')['Total_Sales'].sum()
```

```python
groupedData
```

```python
groupedData.sort_values(ascending=False).head(3)
```

```python
groupedData.nlargest(3)
```

## 2.2 Calculate the average units sold by region

### 3.3 Find all sales that occurred on weekends

```python
sales_data
```

```python
# pd.to_datetime()
```

```python
sales_data['Date'].dt.dayofweek
```

### 4 Create a new column 'Price_Category' that labels each sale as:
     - 'Budget' if Unit_Price < 300
     - 'Mid-Range' if 300 ≤ Unit_Price < 1000
     - 'Premium' if Unit_Price ≥ 1000

```python
sales_data
```

```python
def price_cat(u_price):

  if u_price <300:
    return 'Budget'
  elif u_price >= 300 and u_price < 1000:
    return 'Mid-Range'
  else:
    return 'Premium'

sales_data['Price_Category'] = sales_data.Unit_Price.apply(price_cat)
sales_data
```

## Exercise 2: Employee Data Analysis




```python
import pandas as pd
import numpy as np
```

```python
# Create sample employee data
np.random.seed(42)
departments = ['Engineering', 'Marketing', 'Finance', 'HR', 'Sales']
positions = ['Junior', 'Mid', 'Senior', 'Manager', 'Director']

employee_data = pd.DataFrame({
    'Employee_ID': range(1001, 1051),
    'Name': [f'Employee_{i}' for i in range(1, 51)],
    'Department': np.random.choice(departments, 50),
    'Position': np.random.choice(positions, 50),
    'Salary': np.random.randint(40000, 150000, 50),
    'Join_Date': pd.to_datetime(np.random.choice(pd.date_range('2018-01-01', '2023-01-01'), 50)),
    'Performance_Score': np.random.randint(1, 101, 50)
})
```

```python
employee_data
```

### Tasks:

1. **Basic Operations**:
   - Find all employees in Engineering with salary > $80,000
   - Count the number of employees in each department
   - Find the average salary by position level

2. **Time-Based Analysis**:
   - Calculate employee tenure in years. Suppose the end date is `'2023-01-01'`
   - Find employees who joined in 2020
   - Calculate the average performance score by year of joining

3. **Advanced Grouping**:
   - For each department, find the highest and lowest salary
   - Find departments where the average performance score is above 70

4. **Apply and Transform**:
   - Apply a 5% salary increase to all employees with performance score > 80


```python
# Solution
employee_data
```

### 2.1

```python
((pd.to_datetime('2023-01-01') - employee_data['Join_Date']).dt.days)/365
```

### 4.1

```python
employee_data.loc[employee_data.Performance_Score > 80, 'Salary'] = employee_data.loc[employee_data.Performance_Score > 80, 'Salary']*1.05
```

```python
employee_data
```

```python
employee_data[['Salary', 'Performance_Score']]
```

```python
employee_data.apply?
```

```python
def increase_salary(data):

  if data['Performance_Score'] > 80:
    return data['Salary']*1.05
  else:
    return data['Salary']

employee_data['Performance_Score'].apply(increase_salary)
```

```python
employee_data.apply(
    lambda row :  row['Salary']*1.05 if row['Performance_Score']> 80 else row['Salary'], axis=1
)
```

## Exercise 3: Customer Behavior Analysis


```python
customers = pd.DataFrame({
    'Customer_ID': range(1000, 1020),
    'Age': np.random.randint(18, 70, 20),
    'Gender': np.random.choice(['M', 'F'], 20),
    'Income': np.random.choice(['Low', 'Medium', 'High'], 20, p=[0.4, 0.5, 0.1]),
    'Join_Date': pd.to_datetime(np.random.choice(pd.date_range('2020-01-01', '2023-01-01'), 20))
})

transactions = pd.DataFrame({
    'Transaction_ID': range(5000, 5100),
    'Customer_ID': np.random.choice(customers['Customer_ID'], 100),
    'Date': pd.to_datetime(np.random.choice(pd.date_range('2023-01-01', '2023-03-31'), 100)),
    'Amount': np.random.uniform(5, 500, 100).round(2),
    'Category': np.random.choice(['Electronics', 'Clothing', 'Groceries', 'Entertainment'], 100)
})
```

```python
A = {1, 2, 3}
B = {4, 5, 6}

A , B = {(1, 4),}
```

```python
customers
```

```python
transactions
```

```python
pd.merge?
```


### Tasks:

1. **Data Merging**:
   - Merge customer data with transaction data
   - Find customers with no transactions (left join)

2. **Customer Segmentation**:
   - Group customers by age brackets (18-25, 26-35, etc.) and analyze spending
   - Calculate average transaction amount by income level

```python
# Solution
```

```python
# Left DataFrame (df1)       Right DataFrame (df2)
# +------------+-----+       +------------+-------+
# | customer_id| val |       | customer_id| val2  |
# +------------+-----+       +------------+-------+
# |     1      |  A  |       |     1      |   X   |
# |     2      |  B  |       |     3      |   Y   |
# |     3      |  C  |       |     4      |   Z   |
# +------------+-----+       +------------+-------+
```

```python
df1 = pd.DataFrame({
    'c_id' : [1,2,3],
    'val' : ['A', 'B', 'C']

})

df2 = pd.DataFrame({
    'c_id' : [1,3,4],
    'val' : ['X', 'Y', 'Z']
})
```

```python
df1
```

```python
df2
```

```python
merged_df = pd.merge(df1, df2, how='left', on='c_id')
```

```python
merged_df
```

```python
merged_df[merged_df['val_y'].isna()]
```

```python
pd.merge?
```

```python
merged_data = pd.merge(customers, transactions, how ='left', on='Customer_ID')
```

```python
merged_data
```

```python
merged_data.isna().sum()
```

## ⚠️ Common Beginner Mistakes

- Confusing Machine Learning with simple automation
- Expecting models to be accurate without enough data
- Ignoring data preprocessing


## 📝 Week Summary

- Machine Learning allows systems to learn from data
- ML differs fundamentally from rule-based systems
- This week sets the foundation for upcoming algorithms
