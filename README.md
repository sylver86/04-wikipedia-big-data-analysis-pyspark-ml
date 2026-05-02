# Wikipedia Content Analysis & Classification — PySpark + Databricks

![PySpark](https://img.shields.io/badge/PySpark-3.x-E25A1C?logo=apachespark&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-Platform-FF3621?logo=databricks&logoColor=white)
![NLP](https://img.shields.io/badge/NLP-SparkNLP-4CAF50)
![ML](https://img.shields.io/badge/Classification-Logistic%20Regression-blue)

## Overview

Large-scale EDA and multi-class text classification on Wikipedia articles using Databricks and PySpark. Analyses content across 15 knowledge categories, then trains and compares two Logistic Regression classifiers — one on article **summaries**, one on **full article bodies** — to determine which text signal better predicts category at scale.

Demonstrates end-to-end Big Data NLP: distributed preprocessing with SparkNLP, TF-IDF feature engineering, and multi-class model evaluation using PySpark MLlib.

---

## Dataset

| Attribute | Detail |
|-----------|--------|
| Source | Wikipedia articles (Databricks environment) |
| Fields | `title`, `summary`, `documents`, `categoria` |
| Categories (15) | culture, economics, energy, engineering, finance, humanities, medicine, pets, politics, research, science, sports, technology, trade, transport |

---

## Pipeline

```
Raw Wikipedia CSV
      │
      ▼
SparkNLP Preprocessing
(normalize · lemmatize · remove stopwords)
      │
      ▼
CountVectorizer → TF-IDF
      │
      ▼
┌─────────────────┬────────────────────┐
│ Model A         │ Model B            │
│ summary field   │ documents field    │
│ LogReg (LR)     │ LogReg (LR)        │
└────────┬────────┴────────────────────┘
         │
         ▼
MulticlassClassificationEvaluator
(accuracy · F1 comparison)
```

---

## Analysis Stages

### EDA (PySpark + SparkNLP)

Per-category statistics computed at distributed scale:

| Metric | Description |
|--------|-------------|
| Article count | Number of articles per category |
| Word count stats | Average / max / min words per article |
| TF-IDF top terms | Most representative terms per category |

### Classification Models

| Model | Input | Vectorization | Notes |
|-------|-------|---------------|-------|
| A | `summary` (intro paragraph) | HashingTF | Compact signal, faster training |
| B | `documents` (full article) | HashingTF | Rich context, higher dimensionality |

Both evaluated with `MulticlassClassificationEvaluator` on held-out test split. Full accuracy metrics and per-class results available in the notebook.

---

## Setup

```python
# Load dataset into Databricks environment
!wget https://proai-datasets.s3.eu-west-3.amazonaws.com/wikipedia.csv

import pandas as pd
dataset = pd.read_csv('/databricks/driver/wikipedia.csv')
spark_df = spark.createDataFrame(dataset)
spark_df = spark_df.drop("Unnamed: 0")
spark_df.write.saveAsTable("wikipedia")
```

Open and run `wikipedia_analysis.ipynb` in a Databricks notebook cluster with SparkNLP installed.

---

## Technologies

`PySpark 3.x` · `Databricks` · `Spark NLP` · `PySpark MLlib` · `HashingTF` · `LogisticRegression` · `MulticlassClassificationEvaluator`
