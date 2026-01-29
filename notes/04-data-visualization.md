# 04 - Data Visualization 📊🎨

![pic](assets/data_visualization.png)

> Goal: Visually explore data distributions, relationships, and patterns  
> Tools: `matplotlib` + `seaborn`  

---

## 1. Figure Setup 🖼️

```python
plt.figure(figsize=(16, 6))
```

*   Controls the **overall canvas size**
*   Wide figures are better for time series & comparisons

* * *

2\. Line Plots 📈
-----------------

### 2.1 Plot all numeric columns

```python
sns.lineplot(data=df)
```

*   Automatically plots **all numeric columns**
*   Useful for quick trend comparison

* * *

### 2.2 Plot a single column

```python
sns.lineplot(data=df["col_name"], label="label_name")
```

*   `label` controls legend text

* * *

3\. Bar Plots 📊
----------------

```python
sns.barplot(x=x_col, y=y_col)
```

*   Typically:
    *   `x` → categorical variable
    *   `y` → numeric variable (aggregated automatically)

* * *

4\. Heatmaps 🔥
---------------

```python
sns.heatmap(data=df, annot=True)
```

*   `annot=True` → show values inside each cell
*   Commonly used for:
    *   correlation matrices
    *   confusion matrices

* * *

5\. Scatter & Regression Plots 🔍
---------------------------------

### 5.1 Scatter plot

```python
sns.scatterplot(x=df["x_col"], y=df["y_col"])
```

*   Shows raw relationship between two variables

* * *

### 5.2 Scatter plot with regression line

```python
sns.regplot(x=df["x_col"], y=df["y_col"])
```

*   Adds a **linear regression trend**

* * *

6\. Hue: Color Encoding 🎨
--------------------------

```python
sns.scatterplot(
    x=df["x_col"],
    y=df["y_col"],
    hue=df["hue_col"]
)
```

**Behavior**

*   Categorical data → different colors per category
*   Numeric data → gradient color scale

* * *

7\. Multiple Regression Lines by Category 📐
--------------------------------------------

```python
sns.lmplot(
    x="x_col",
    y="y_col",
    hue="category_col",
    data=df
)
```

**Important ⚠️**

*   Column names are passed as **strings**
*   `lmplot` handles faceting & legends automatically

* * *

8\. Swarm Plot (Distribution Details) 🐝
----------------------------------------

```python
sns.swarmplot(x="category_col", y="value_col", data=df)
```

*   Prevents point overlap
*   Shows **fine-grained distribution** per category
*   Best for small-to-medium datasets

* * *

9\. Quick Data Inspection 👀
----------------------------

```python
df.head()   # first 5 rows
df.tail()   # last 5 rows
```

* * *

10\. Reading Time-Series Data ⏱️
--------------------------------

```python
df = pd.read_csv(
    "data.csv",
    index_col="date",
    parse_dates=True
)
```

*   Parses index directly as `datetime`

* * *

### Access index values after setting index

```python
df.index
```

*   Use this when the original column is now the index

11\. Distributions 📊📐
---------------------------

### 11.1 Histogram (Frequency Distribution)

```python
sns.histplot(df["col_name"])
```

*   Shows the **frequency distribution** of a single variable
*   Useful for checking skewness & spread

### 11.2 Histogram with categories

```
sns.histplot(
    data=df,
    x="col_name",
    hue="category_col"
)
```

*   `hue` splits the data into **multiple colored distributions**
*   Great for comparing groups

12\. Density Plots (KDE) 🌊
---------------------------

### 12.1 Single-variable KDE

### 

```python
sns.kdeplot(df["col_name"], shade=True)
```

*   Smooth estimation of the distribution
*   `shade=True` fills the area under the curve

### 12.2 KDE with categories

```python
sns.kdeplot(
    data=df,
    x="col_name",
    hue="category_col",
    shade=True
)
```

*   Overlapping density curves by category

* * *

13\. Joint Distributions 🔗
---------------------------

### 13.1 2D KDE (Joint Plot)

```python
sns.jointplot(
    x=df["x_col"],
    y=df["y_col"],
    kind="kde"
)
```

*   Visualizes the **joint density** of two variables
*   Useful for spotting clusters & correlations

* * *

14\. Seaborn Styles 🎨
----------------------

```python
sns.set_style("dark")
```

Available styles:

*   `darkgrid`
*   `whitegrid`
*   `dark`
*   `white`
*   `ticks`