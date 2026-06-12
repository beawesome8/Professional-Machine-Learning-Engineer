
# 🛒 Predict Visitor Purchases with BigQuery ML

## Overview
Used **BigQuery Machine Learning (BigQuery ML)** to build a model that predicts whether a visitor to the Google Merchandise Store will make a transaction — all using SQL, no Python or separate ML tools required.

The dataset used was a public Google Analytics sample dataset containing millions of real session records, loaded directly into BigQuery.

---

## What I Learned
- How to explore and prepare data for machine learning in BigQuery
- How to create a logistic regression model using SQL
- How to evaluate model performance using `ML.EVALUATE`
- How to make predictions using `ML.PREDICT`
- How to use Gemini Code Assist to explain, generate, and debug SQL queries

---

## Tools & Services Used
- Google BigQuery
- BigQuery ML
- Gemini Code Assist (AI-assisted SQL generation & debugging)
- Google Cloud Console
- SQL

---

## Tasks Completed

### Task 1 — Explore the Data
Queried the public Google Analytics dataset to inspect session records.
Selected key features relevant to predicting transactions:

| Feature | Description |
|---------|-------------|
| `label` | 1 if transaction occurred, 0 if not |
| `os` | Visitor's operating system |
| `is_mobile` | Whether the visitor used a mobile device |
| `country` | Visitor's country |
| `pageviews` | Number of pages viewed in the session |

Saved the result as a **view** called `training_data` in the `bqml_lab` dataset.

---

### Task 2 — Create the Model
Used Gemini's SQL generation tool to create a **binary logistic regression model** trained on `training_data`.

```sql
CREATE MODEL `bqml_lab.sample_model`
OPTIONS (
  model_type = 'LOGISTIC_REG',
  input_label_cols = ['label']
) AS
SELECT
  label,
  os,
  is_mobile,
  country,
  pageviews
FROM
  `bqml_lab.training_data`;
```

- **Model type:** Logistic Regression (binary classification)
- **Goal:** Predict whether a visitor will make a purchase (1) or not (0)

---

### Task 3 — Evaluate the Model
Used `ML.EVALUATE` to assess model performance against the training data.

```sql
SELECT *
FROM ML.EVALUATE(
  MODEL `bqml_lab.sample_model`,
  TABLE `bqml_lab.training_data`
);
```

Key metrics returned included precision, recall, accuracy, and AUC (Area Under Curve).

---

### Task 4 — Use the Model to Make Predictions
Created a new view `july_data` using July 2017 session records, then used `ML.PREDICT` to predict purchases.

**Top 10 countries by predicted purchases:**

```sql
SELECT
  country,
  SUM(predicted_label) AS total_predicted_purchases
FROM
  ML.PREDICT(MODEL `bqml_lab.sample_model`, (
    SELECT * FROM `bqml_lab.july_data`
  ))
GROUP BY country
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```

**Bug encountered & fixed:**
An initial query used `TOTAL()` which is not a valid BigQuery function.
Used Gemini to debug the error — Gemini correctly identified that `SUM()` should be used instead.

---

## Key Takeaways
- BigQuery ML makes it possible to train and run ML models entirely in SQL — no separate ML environment needed
- Gemini Code Assist is a powerful tool for explaining unfamiliar queries, generating boilerplate SQL, and debugging errors quickly
- Logistic regression is well suited for binary classification problems like "will this user buy or not?"
- Feature selection matters — the model used operating system, device type, country, and pageviews as predictors

---

## Dataset
- **Source:** `bigquery-public-data.google_analytics_sample.ga_sessions_*`
- **Training period:** August 2016 – June 2017
- **Prediction period:** July 2017

---

*Part of my Google Cloud Platform learning journey.*
