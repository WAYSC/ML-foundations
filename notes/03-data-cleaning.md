# 03 · Data Cleaning & Preprocessing 🧹🧪

> Goal: Handle **missing values, scaling, dates, encoding, and messy strings**  
> Focus: Real-world data issues before modeling  
> Status: ✅ Core data cleaning toolkit

---

## 1. Missing Values (NaN) 🚨

### 1.1 Count missing values per column

```python
df.isnull().sum()
```
Returns the number of missing values for each column

### 1.2 Count missing values in the entire DataFrame

```python
df.isnull().sum().sum()
```

- First sum() → per column
- Second sum() → whole table

### 1.3 Drop rows with missing values

```python
df.dropna()
```
Drops rows containing at least one missing value

### 1.4 Drop columns with missing values

```python
df.dropna(axis=1)
```
Drops columns that have any missing value

### 1.5 Fill missing values with a constant

```python
df.fillna(0)
```

### 1.6 Backfill missing values + fallback

```python
df.fillna(method="bfill", axis=0).fillna(0)
```

Logic 🧠

- Use the next value in the same column to fill NaN
- If no next value exists → fill with 0

## 2. Feature Scaling & Transformation 📏

### 2.1 Min-Max Scaling

```python
minmax_scaling(df, columns=["col_name"])
```

- Scales values to [0, 1]
- Keeps relative distances

### 2.2 Normalization (Box-Cox)

```python
from scipy import stats
stats.boxcox(data)
```

- Makes data more normally distributed
- Input values must be positive

## 3. Date & Time Processing ⏰
### 3.1 Convert string to datetime

```python
pd.to_datetime(df["date"], format="%m/%d/%y")
```

Explicit format avoids ambiguity and errors

### 3.2 Extract day from datetime

```python
df["date"].dt.day
```

.dt accessor is required for datetime operations

## 4. Text Encoding & Decoding 🔤
### 4.1 Decode using original encoding

```python
sample.decode("original_encoding")
```

### 4.2 Encode to UTF-8

```python
sample.encode("utf-8")
```

Typical workflow:

- Decode with the source encoding
- Re-encode using UTF-8

### 4.3 Save files with UTF-8 encoding

```python
df.to_csv("output.csv", encoding="utf-8")
```

## 5. Fuzzy String Matching 🧩
### 5.1 Approximate string matching

```python
from fuzzywuzzy import process, fuzz

process.extract(
    query_string,
    choices_list,
    limit=5,
    scorer=fuzz.token_sort_ratio
)
```
Output:
```text
[(matched_text, similarity_score)]
```

Use cases:

- Fix typos
- Match inconsistent names
- Deduplicate messy text fields

✅ End of Notes 03