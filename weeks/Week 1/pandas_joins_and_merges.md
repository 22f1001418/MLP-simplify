
# Joins and Merges in Pandas

## Introduction

If data manipulation is about cleaning and shaping data, then joins are about relationships.

In the real world, data rarely lives in one perfect table. Instead, it is scattered across multiple tables such as student details, marks, attendance, and results. To make sense of the full picture, we must combine these tables correctly.

This chapter teaches joins and merges in Pandas from absolute zero. No prior database or SQL knowledge is assumed.

---

## The Running Example

We will use the same situation throughout this chapter.

### Students Table

```python
import pandas as pd

students = pd.DataFrame({
    "student_id": [1, 2, 3, 4],
    "name": ["Amit", "Neha", "Ravi", "Sara"]
})
```

| student_id | name |
|-----------|------|
| 1 | Amit |
| 2 | Neha |
| 3 | Ravi |
| 4 | Sara |

### Marks Table

```python
marks = pd.DataFrame({
    "student_id": [1, 2, 3, 5],
    "marks": [78, 85, 92, 66]
})
```

| student_id | marks |
|-----------|-------|
| 1 | 78 |
| 2 | 85 |
| 3 | 92 |
| 5 | 66 |

Notice:
- Student 4 has no marks
- Student 5 has marks but no student record

This mismatch is intentional. Real datasets are imperfect.

---

## What Is a Join or Merge?

A join means combining two tables based on a common column called a key.

In Pandas, this is done using the merge function.

Think of a join as matching rows using a key and stitching columns together.

---

## Inner Join

Keeps only rows where the key exists in both tables.

```python
pd.merge(students, marks, on="student_id", how="inner")
```

Output:

| student_id | name | marks |
|-----------|------|-------|
| 1 | Amit | 78 |
| 2 | Neha | 85 |
| 3 | Ravi | 92 |

Explanation:
- Student 4 is removed
- Student 5 is removed

Use inner join when you want only complete records.

---

## Left Join

Keeps all rows from the left table.

```python
pd.merge(students, marks, on="student_id", how="left")
```

Output:

| student_id | name | marks |
|-----------|------|-------|
| 1 | Amit | 78 |
| 2 | Neha | 85 |
| 3 | Ravi | 92 |
| 4 | Sara | NaN |

Missing values appear as NaN.

---

## Right Join

Keeps all rows from the right table.

```python
pd.merge(students, marks, on="student_id", how="right")
```

Output:

| student_id | name | marks |
|-----------|------|-------|
| 1 | Amit | 78 |
| 2 | Neha | 85 |
| 3 | Ravi | 92 |
| 5 | NaN | 66 |

---

## Outer Join

Keeps all rows from both tables.

```python
pd.merge(students, marks, on="student_id", how="outer")
```

Output:

| student_id | name | marks |
|-----------|------|-------|
| 1 | Amit | 78 |
| 2 | Neha | 85 |
| 3 | Ravi | 92 |
| 4 | Sara | NaN |
| 5 | NaN | 66 |

---

## Joining on Different Column Names

```python
pd.merge(students, marks, left_on="student_id", right_on="student_id")
```

If column names differ, use left_on and right_on explicitly.

---

## Joining on Index

```python
students.set_index("student_id", inplace=True)
marks.set_index("student_id", inplace=True)

students.join(marks)
```

Use this when the index represents the join key.

---

## Duplicate Keys

Duplicate keys can multiply rows during joins.

Always check for duplicates before merging.

---

## Common Pitfalls

- Using the wrong join type
- Ignoring NaN values after join
- Joining on the wrong column
- Unexpected row duplication

---

## Summary

- merge is used to combine tables
- Join type controls row retention
- NaN values are normal after joins
- Always inspect results

---

## Video Explanation

Add your lecture recording link here.
