# 📊 Big Data Databricks & PySpark: Wikipedia Content Analysis and Classification

## 📋 Project Overview

This project involves an extensive Exploratory Data Analysis (EDA) and text classification task on Wikipedia data using Databricks and PySpark. As a Data Scientist for Wikimedia, the goal is to statistically analyze and evaluate the informational content offered by Wikipedia across various categories.

## 🎯 Objectives

1. Perform EDA on Wikipedia data
2. Develop and test a text classifier for future article categorization

## 📚 Dataset Description

The dataset contains Wikipedia articles with the following columns:
- **title**: Article title
- **summary**: Article introduction
- **documents**: Full article content
- **categoria**: Associated category

Categories include: culture, economics, energy, engineering, finance, humanities, medicine, pets, politics, research, science, sports, technology, trade, transport

## 🔍 EDA Process

The Exploratory Data Analysis was conducted using PySpark and included the following steps:

1. Data Cleaning: Used SparkNLP to normalize text, lemmatize words, and remove stopwords.
2. Text Vectorization: Implemented CountVectorizer to convert text into numerical features.
3. Statistical Analysis: For each category, calculated:
   - Number of articles
   - Average number of words used
   - Maximum number of words in the longest article
   - Minimum number of words in the shortest article
4. TF-IDF Analysis: Applied TF-IDF (Term Frequency-Inverse Document Frequency) to identify the most representative words for each category.

## 🤖 Classification Model

Two separate logistic regression models were created, one for the "summary" field and another for the "documents" field:

1. Data Preparation:
   - Split the dataset into training and test sets
   - Used HashingTF for feature vectorization

2. Model Training:
   - Implemented Logistic Regression using PySpark's ML library
   - Trained the model on the vectorized data

3. Model Evaluation:
   - Used MulticlassClassificationEvaluator to assess model performance
   - Calculated accuracy and other relevant metrics

The models were compared to determine which field (summary or documents) provided better classification accuracy.

## 🛠️ Technologies Used

- Databricks
- PySpark
- Spark NLP
- PySpark ML (for machine learning tasks)

## 🔧 Setup

To load the dataset and create a Spark table:

```python
!wget https://proai-datasets.s3.eu-west-3.amazonaws.com/wikipedia.csv
import pandas as pd
dataset = pd.read_csv('/databricks/driver/wikipedia.csv')
spark_df = spark.createDataFrame(dataset)
spark_df = spark_df.drop("Unnamed: 0")
spark_df.write.saveAsTable("wikipedia")
```

🔗 **GitHub Repository:**
Dive into the codebase (files .ipynb).

## 🤝 Contributing

Contributions, issues, and feature requests are welcome. 
