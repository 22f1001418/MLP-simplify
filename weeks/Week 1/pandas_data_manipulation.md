
# Data Manipulation Using Pandas

## Introduction

Data analysis without Pandas is like trying to cook a full meal using only a spoon. Possible, but unnecessarily painful.

Pandas is the backbone of data manipulation in Python. Almost every Machine Learning or Data Science pipeline begins with Pandas, not because it is fancy, but because it is practical. It helps us read data, understand it, clean it, reshape it, and ask meaningful questions from it.

This chapter is written like a book. Read it sequentially. Each concept builds on the previous one. By the end, Pandas should feel less like magic and more like muscle memory.

---

## Why Do We Need Pandas?

Let us start with a simple question.

Suppose you have student marks stored like this:

```python
marks = [78, 85, 92, 67]
```

Now imagine you also have names, roll numbers, attendance, grades, and this data is coming from a CSV file with thousands of rows.

At this point:
- Lists become unmanageable
- Dictionaries become messy
- NumPy arrays lose column meaning

Pandas exists to solve exactly this problem.

It allows us to:
- Work with data in tabular form
- Refer to columns by name, not by index
- Perform operations on entire columns at once
- Write expressive, readable code

Think of Pandas as Excel inside Python, but without the clicking.

---

## Series and DataFrame

### Series

A Series is a one-dimensional data structure. Think of it as a single column of a table.

```python
import pandas as pd

scores = pd.Series([45, 67, 89, 90])
print(scores)
```

Output:
```text
0    45
1    67
2    89
3    90
dtype: int64
```

Notice two things:
- Every value has an index
- The data type is stored

You can also provide your own index.

```python
scores = pd.Series([45, 67, 89], index=["A", "B", "C"])
```

Output:
```text
A    45
B    67
C    89
dtype: int64
```

Common mistake:
Assuming a Series behaves exactly like a list. It does not. Indexing and operations are label-aware.

---

### DataFrame

A DataFrame is a two-dimensional table made up of rows and columns.

```python
data = {
    "Name": ["Amit", "Neha", "Ravi"],
    "Marks": [78, 85, 92],
    "Passed": [True, True, True]
}

df = pd.DataFrame(data)
print(df)
```

Output:
```text
   Name  Marks  Passed
0  Amit     78    True
1  Neha     85    True
2  Ravi     92    True
```

Each column in a DataFrame is a Series.

Mental model:
A DataFrame is a collection of Series sharing the same index.

---

## Creating DataFrames

### From dictionaries

```python
df = pd.DataFrame({
    "City": ["Delhi", "Mumbai", "Chennai"],
    "Population": [19, 20, 10]
})
```

Output:
```text
      City  Population
0    Delhi          19
1   Mumbai          20
2  Chennai          10
```

### From list of dictionaries

```python
data = [
    {"Name": "A", "Score": 90},
    {"Name": "B", "Score": 85},
    {"Name": "C", "Score": 88}
]

df = pd.DataFrame(data)
```

Edge case:
If one dictionary is missing a key, Pandas fills it with NaN. Pandas is polite, not judgmental.

---

## Reading Data from Files

### Reading CSV

```python
df = pd.read_csv("students.csv")
```

Typical issues:
- File path incorrect
- Wrong separator
- Unexpected encoding

Always inspect after reading.

### Reading Excel

```python
df = pd.read_excel("students.xlsx")
```

---

## Inspecting Data

Before touching the data, look at it. Always.

### head()

```python
df.head()
```

Output:
Shows the first 5 rows by default.

### info()

```python
df.info()
```

Output includes:
- Column names
- Data types
- Non-null counts

This is where you discover silent problems.

### describe()

```python
df.describe()
```

Output:
- Mean, median, min, max, quartiles

If describe surprises you, your data has already started lying.

---

## Indexing and Selection

### Selecting columns

```python
df["Marks"]
```

Returns a Series.

```python
df[["Name", "Marks"]]
```

Returns a DataFrame.

This difference matters.

---

### loc

Label-based indexing.

```python
df.loc[1, "Marks"]
```

Means:
Row with label 1, column Marks.

---

### iloc

Position-based indexing.

```python
df.iloc[1, 1]
```

Means:
Second row, second column.

Rule of survival:
Use loc when possible. Use iloc when unavoidable.

---

## Filtering Rows

Filtering means asking questions.

```python
df[df["Marks"] > 80]
```

Output:
Rows where Marks is greater than 80.

Multiple conditions:

```python
df[(df["Marks"] > 80) & (df["Passed"] == True)]
```

Mistake that everyone makes once:
Forgetting parentheses. Pandas will remind you, rudely.

---

## Adding and Removing Columns

### Adding columns

```python
df["Grade"] = ["B", "A", "A"]
```

Or dynamically:

```python
df["Status"] = df["Marks"].apply(lambda x: "Pass" if x >= 40 else "Fail")
```

### Dropping columns

```python
df.drop("Grade", axis=1, inplace=True)
```

Axis matters:
axis=1 means column, axis=0 means row.

---

## Statistical Operations

```python
df["Marks"].mean()
df["Marks"].median()
df["Marks"].sum()
df["Marks"].min()
df["Marks"].max()
```

All these skip missing values automatically.

Silent feature. Very useful. Occasionally dangerous.

---

## value_counts and unique

```python
df["Grade"].value_counts()
```

Output:
Counts frequency of each category.

```python
df["Grade"].unique()
```

Output:
Array of unique values.

Use these when categories confuse you.

---

## GroupBy

groupby is where Pandas stops being simple and starts being powerful.

```python
df.groupby("Grade")["Marks"].mean()
```

Mental steps:
- Split by Grade
- Apply mean on Marks
- Combine results

If groupby feels confusing, slow down. This is a turning point topic.

---

## Conditional Logic

```python
df[df["Marks"] >= 60]
```

Used for filtering.

Also used for column creation.

```python
df["Result"] = df["Marks"].apply(
    lambda x: "Excellent" if x >= 85 else "Average"
)
```

---

## The apply Function

apply lets you run custom Python logic on Pandas objects.

```python
def performance(score):
    if score >= 85:
        return "Excellent"
    elif score >= 60:
        return "Good"
    else:
        return "Needs Improvement"

df["Performance"] = df["Marks"].apply(performance)
```

Warning:
apply is powerful, but slow. Use it wisely.

---

## Common Mistakes

- Editing data without inspecting it
- Mixing loc and iloc
- Forgetting axis
- Overusing apply
- Assuming data has no missing values

---

## Summary

Pandas is not difficult. It is expressive.

Once you stop thinking row by row and start thinking column by column, Pandas becomes intuitive. This chapter equips you with enough tools to confidently manipulate real-world datasets and prepares you for data preprocessing in Machine Learning.

---

## Video Explanation

Add your lecture recording link here.
