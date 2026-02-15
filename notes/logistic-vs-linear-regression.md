# 06: Logistic Regression ≠ Linear Regression

It is a common rite of passage for every data scientist to be confused by the name **Logistic Regression**. Despite the word "Regression" in its name, it is a **classification** algorithm. 

This note explains the fundamental differences in their principles, mathematical foundations, and use cases.

---

## 1. The Core Difference
While both are supervised learning models and part of the Generalized Linear Model (GLM) family, they serve different purposes:

* **Linear Regression:** Predicts a **continuous** numeric value (e.g., house prices, temperature).
* **Logistic Regression:** Predicts the **probability** of a categorical outcome (e.g., Spam vs. Not Spam, Yes vs. No).

---

## 2. Mathematical Principles

### Linear Regression
Linear regression assumes a linear relationship between the input variables ($x$) and the output ($y$). The output can range from negative infinity to positive infinity.

The equation is:
$$y = \beta_0 + \beta_1x_1 + \beta_2x_2 + ... + \beta_nx_n$$

### Logistic Regression
In classification, we need a value between 0 and 1. To achieve this, Logistic Regression wraps the linear equation inside a **Sigmoid Function** (also called the Logistic Function).


The Sigmoid function is defined as:
$$\sigma(z) = \frac{1}{1 + e^{-z}}$$

By substituting the linear equation $z = \beta^T x$ into the sigmoid function, we get:
$$P(y=1|x) = \frac{1}{1 + e^{-(\beta_0 + \beta_1x)}}$$

---

## 3. Comparison Table

| Feature | Linear Regression | Logistic Regression |
| :--- | :--- | :--- |
| **Output Type** | Continuous (Number) | Probability (0 to 1) |
| **Goal** | Minimize distance to points | Separate the classes |
| **Loss Function** | Mean Squared Error (MSE) | Log Loss (Cross-Entropy) |
| **Activation** | Identity Function | Sigmoid / Logistic Function |
| **Linearity** | Linear relationship | Linear relationship in *log-odds* |

---

## 4. Why "Regression" in the name?
It’s called regression because it technically "regresses" the **Log-Odds** (Logit) of the probability. 

The relationship is expressed as:
$$\ln\left(\frac{p}{1-p}\right) = \beta_0 + \beta_1x$$

The left side of this equation is the **Logit link function**. While the output is a class, the underlying model is still fitting a linear combination of features to a continuous value (the log-odds).

---

## 5. Summary
* Use **Linear Regression** when you want to answer "How much?"
* Use **Logistic Regression** when you want to answer "Which one?" (specifically for binary outcomes).