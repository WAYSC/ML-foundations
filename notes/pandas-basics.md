# 02 - Pandas Basics 🐼📊


---

## 1. DataFrame & Series Basics 🧱

### 1.1 Create a DataFrame from a dictionary

```python
df = pd.DataFrame({
    "col1": [1, 2, 3],
    "col2": [4, 5, 6]
})
```

⚠️ Key rules 

- Dictionary keys = column names

- Dictionary values must be iterable (lists, not scalars)

- ❌ This is invalid:

```python
pd.DataFrame({"A": 1})
```

### 1.2 Custom row index

```python
df = pd.DataFrame(
    {"A": [10, 20], "B": [30, 40]},
    index=["row1", "row2"]
)
```

### 1.3 Series (1D data structure)

```python
s = pd.Series(
    [10, 20, 30],
    index=["a", "b", "c"],
    name="score"
)
```

Mental model 🧠

- Series = a single column without a table frame

- First argument: data list

- index: row labels

- name: column name

## 2. Reading Data 📂

### 2.1 Read CSV

```python
df = pd.read_csv("data.csv")
```

Use a column as index:

```python
df = pd.read_csv("data.csv", index_col="id")
```

### 2.2 Quick inspection

```python
df.head()   # first 5 rows by default
```

3. Core DataFrame Understanding 🔍

- DataFrame is 2D: rows first, then columns

- Each row = one sample / record

- Think in terms of:

    samples (rows) × features (columns)

## 4. Selecting Rows & Columns 🎯

### 4.1 Select a column

```python
df.col_name
df["col_name"]
```

> Feels like accessing an attribute or feature

### 4.2 iloc — position-based indexing

Rule: numbers only 🔢

```python
df.iloc[0]        # first row
df.iloc[:, 0]     # all rows, first column
df.iloc[[0, 2], [1, 3]]
```

Notes:

- Slicing does not require extra []

- iloc slicing is left-closed, right-open

### 4.3 loc — label-based indexing

```python
df.loc["row1", "colA"]
df.loc[["row1", "row2"], ["colA", "colB"]]
```

⚠️ Important difference 

- Uses row/column names

- Slice ranges are inclusive on both ends

## 5. Conditional Selection (Boolean Filtering) 🔎

### 5.1 Basic condition

```python
df.loc[df["price"] > 100]
```

### 5.2 Multiple conditions

```python
df.loc[
    (df["price"] > 100) & (df["country"] == "US")
]
```

Rules:

- & = AND, | = OR

- Each condition must be wrapped in parentheses

### 5.3 Useful helpers

```python
df["country"].isin(["US", "UK"])
df["price"].notnull()
```

## 6. Assigning Values to Columns ✍️

```python
df["new_col"] = range(len(df))
```

Rules:

- Assigned values must be iterable

- Length must match the number of rows

## 7. Common Statistics & Exploration 📈

### 7.1 describe()

```python
df["price"].describe()
```

Returns:

- count / mean / std / min / max / percentiles

### 7.2 Column-wise stats

```python
df["price"].mean()
df["country"].unique()
df["country"].value_counts()
```

## 8. map vs apply 🔄

### 8.1 map — element-wise

```python
df["price"].map(lambda p: p * 2)
```

Count values satisfying a condition:

```python
(df["price"].map(lambda p: p > 100)).sum()
```

### 8.2 apply — row-wise operations

```python
def score_row(row):
    return row["A"] + row["B"]

df.apply(score_row, axis="columns")
```

Key idea

- axis="columns" → function receives each row

- Best for multi-column logic

## 9. idxmax() 🏆

```python
df["price"].idxmax()
```

Purpose

- Returns the index of the maximum value

- Common use: find the best / largest / most expensive record

## 10. `groupby` Basics 🧩

### 10.1 `groupby + count` vs `value_counts`

```python
df.groupby(["category"]).category.count()
```

is equivalent to:

```python
df["category"].value_counts()
```

Key idea 🧠

- groupby(列名) → that column becomes the Series index
    
- Aggregated results become the values

### 10.2 Grouped column as the first column

```python
df.groupby("country")["price"].mean()
```

- country → index (first column conceptually)
	
- Aggregation results → following columns / values

## 11. Multiple Aggregations with agg 🧮

```python
df.groupby("country")["price"].agg(["mean", "max", "min"])
```

Rules

- Each function runs independently

- Output column names = function names

- Very common in EDA & reporting 📑

## 12. Sorting Data 🔃

```python
df.sort_values(by=["price"])
```

Descending order:

```python
df.sort_values(by=["price"], ascending=False)
```

Default: ascending=True

## 13. count vs size ⚖️

```python
df.groupby("country").count()
df.groupby("country").size()
```

Difference

- count() → counts non-null values only

- size() → counts all rows (including nulls)

## 14. Aggregation with sum ➕

```python
df.groupby("country")["sales"].sum()
```

Sums numeric values within each group

## 15. Data Types & Type Conversion 🧬

### 15.1 Check data type

```python
df["price"].dtype
```

⚠️ No parentheses

### 15.2 Convert data type

```python
df["price"] = df["price"].astype(float)
```

## 16. Missing Values 🚨

### 16.1 Detect null values

```python
pd.isnull(df["price"])
```

- Returns a boolean Series

- True → missing value

- False → non-missing value

### 16.2 Count missing values

```python
pd.isnull(df["price"]).sum()
```

### 16.3 Fill missing values

```python
df["price"].fillna(0)
df["price"].fillna("unknown")
```

## 17. Renaming Labels ✏️

### 17.1 Rename columns

```python
df.rename(columns={"old_name": "new_name"})
```

### 17.2 Rename index (rows)

```python 
df.rename(index={"old_index": "new_index"})
```

### 17.3 Rename axis labels

```python
df.rename_axis("row_name", axis="rows")
df.rename_axis("column_name", axis="columns")
```

## 18. Combining DataFrames 🔗

### 18.1 concat (same columns)

```python
pd.concat([df1, df2])
```

- Works best when column names are identical

### 18.2 join with multiple indexes 🔑

Set indexes first:

```python
left = left.set_index(["key1", "key2"])
right = right.set_index(["key1", "key2"])
```

Join on common index values:

```python
left.join(
    right,
    lsuffix="_left",
    rsuffix="_right"
)
```

Key idea

- Only rows with matching multi-index keys are joined

- Suffixes avoid column name conflicts


✅ End of Part 2

🎯 Pandas core operations complete