# 05 - Feature Engineering 🛠️🧠

> Goal: Transform raw data into **model-ready, high-signal features**  
> Focus: relevance, aggregation, clustering, dimensionality reduction, encoding  
> Status: ✅ Core techniques covered

---

## 1. Mutual Information (Feature Relevance) 🔍

Mutual Information (MI) measures **how much information a feature provides about the target**,  
capturing **non-linear relationships** beyond simple correlation.

### 1.1 MI score helper function

```python
from sklearn.feature_selection import mutual_info_regression

def make_mi_scores(X, y):
    X = X.copy()

    # Convert categorical features to integer codes
    for colname in X.select_dtypes(["object", "category"]):
        X[colname], _ = X[colname].factorize()

    # Identify discrete features
    discrete_features = [
        pd.api.types.is_integer_dtype(t) for t in X.dtypes
    ]

    mi_scores = mutual_info_regression(
        X, y,
        discrete_features=discrete_features,
        random_state=0
    )

    mi_scores = pd.Series(
        mi_scores,
        name="MI Scores",
        index=X.columns
    ).sort_values(ascending=False)

    return mi_scores
````

* * *

### 1.2 Separate features and target

```python
X = df.copy()
y = X.pop("SalePrice")  # remove target column
```

* * *

### 1.3 Compute MI scores

```python
mi_scores = make_mi_scores(X, y)
```

*   Higher score → more predictive power
*   MI = 0 → feature is likely useless 🚫

* * *

2\. Row-wise Feature Aggregation ➕
----------------------------------

### Count values greater than 0 across selected columns

```python
df["new_feature"] = df[cols].gt(0).sum(axis=1)
```

*   `.gt(0)` → boolean mask
*   `.sum(axis=1)` → count `True` per row

* * *

3\. String Feature Expansion 🔤
-------------------------------

### Split a column into multiple features

```python
df[["part1", "part2"]] = df["col_name"].str.split(
    "separator",
    expand=True
)
```

*   Useful for structured strings (IDs, categories, ranges)

* * *

4\. Group-based Statistical Features 📊
---------------------------------------

### Group-wise transformation

```python
df["group_stat"] = (
    df.groupby("group_col")["value_col"]
      .transform("mean")
)
```

Common aggregations:

*   `mean`
*   `median`
*   `max`
*   `min`

🧠 Keeps original row count (unlike `agg`)

* * *

5\. Clustering as a Feature (KMeans) 🧩
---------------------------------------

### 5.1 Fit KMeans and assign cluster labels

```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=k)
df["Cluster"] = kmeans.fit_predict(df[features])
df["Cluster"] = df["Cluster"].astype("category")
```

*   Clusters capture **latent structure**
*   Often very powerful in tabular data

* * *

### 5.2 Visualize clusters (scatter)

```python
sns.relplot(
    x="x_col",
    y="y_col",
    hue="Cluster",
    data=df
)
```

* * *

### 5.3 Compare clusters with boxen plots

```python
sns.catplot(
    x="Cluster",
    y="value_col",
    data=df,
    kind="boxen"
)
```

*   Boxen plot shows **distribution tails** better than boxplot

* * *

6\. PCA (Dimensionality Reduction) 🧬
-------------------------------------

⚠️ **Always standardize before PCA**, otherwise scale dominates.

### 6.1 Standardization

```python
X_scaled = (X - X.mean(axis=0)) / X.std(axis=0)
```

* * *

### 6.2 Apply PCA

```python
from sklearn.decomposition import PCA

pca = PCA()
X_pca = pca.fit_transform(X_scaled)
```

*   Reduces dimensionality
*   Removes multicollinearity
*   Captures maximum variance

* * *

7\. Target Encoding (Advanced Categorical Encoding) 🎯
------------------------------------------------------

### 7.1 Define encoder

```python
from category_encoders import MEstimateEncoder

encoder = MEstimateEncoder(
    cols=["Zipcode"],
    m=5.0
)
```

* * *

### 7.2 Fit encoder (encoding set only)

```python
encoder.fit(X_encode, y_encode)
```

*   Learns relationship between category and target
*   Uses **regularization** (`m`) to reduce overfitting

* * *

### 7.3 Transform training data

```python
X_train = encoder.transform(X_pretrain)
```

* * *

### 7.4 Why target encoding matters 💡

*   Converts **non-numeric categories** into **target-aware continuous values**
*   Strongly correlated with the target variable (e.g. Rating ⭐)
*   Mitigates:
    *   overfitting
    *   noisy rare categories
    *   high-cardinality explosions

✅ Final output: **clean, numeric, high-signal features** ready for ML models

* * *