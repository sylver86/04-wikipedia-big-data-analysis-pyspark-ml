# WikiClassifier — Analisi Big Data e Classificazione Testuale con PySpark

![PySpark](https://img.shields.io/badge/PySpark-3.x-E25A1C?logo=apachespark&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-Platform-FF3621?logo=databricks&logoColor=white)
![SparkNLP](https://img.shields.io/badge/NLP-SparkNLP-4CAF50)
![ML](https://img.shields.io/badge/Classification-Logistic%20Regression-blue)

## Panoramica

Analisi esplorativa e classificazione testuale multi-classe su articoli Wikipedia con Apache Spark su Databricks. Il progetto analizza contenuti su 15 categorie tematiche e confronta due classificatori Logistic Regression — uno sul campo `summary` e uno sul corpo completo dell'articolo (`documents`) — per determinare quale segnale testuale ottimizza la categorizzazione a scala distribuita.

Competenze applicabili a sistemi di content intelligence, categorizzazione automatica documenti, customer segmentation testuale e fraud detection in contesti enterprise.


## Dataset

| Attributo | Dettaglio |
|-----------|-----------|
| Sorgente | Wikipedia (via Databricks) |
| Campi | `title`, `summary`, `documents`, `categoria` |
| Categorie (15) | culture, economics, energy, engineering, finance, humanities, medicine, pets, politics, research, science, sports, technology, trade, transport |

## Pipeline

```
Dataset Wikipedia CSV
       │
       ▼
SparkNLP Preprocessing
(normalizzazione · lemmatizzazione · rimozione stopwords)
       │
       ▼
CountVectorizer → TF-IDF
       │
       ▼
┌─────────────────────┬─────────────────────┐
│ Modello A           │ Modello B            │
│ campo: summary      │ campo: documents     │
│ Logistic Regression │ Logistic Regression  │
└──────────┬──────────┴──────────────────────┘
           │
           ▼
MulticlassClassificationEvaluator
(accuracy · F1 a confronto)
```

## Fasi di Analisi

| Fase | Tecnica | Output |
|------|---------|--------|
| EDA | PySpark + SparkNLP | Statistiche per categoria (conteggio articoli, lunghezza, top TF-IDF terms) |
| Feature Engineering | CountVectorizer + HashingTF | Feature vettorizzate distribuite |
| Classificazione | Logistic Regression (×2) | Accuracy su test set per `summary` vs `documents` |
| Valutazione | MulticlassClassificationEvaluator | Comparazione accuracy e F1 tra i due modelli |

## Setup (Databricks)

```python
# Caricamento dataset nell'ambiente Databricks
!wget https://proai-datasets.s3.eu-west-3.amazonaws.com/wikipedia.csv
import pandas as pd
dataset = pd.read_csv('/databricks/driver/wikipedia.csv')
spark_df = spark.createDataFrame(dataset).drop("Unnamed: 0")
spark_df.write.saveAsTable("wikipedia")
```

Aprire ed eseguire i notebook nella cartella `notebooks/` su un cluster Databricks con SparkNLP installato.

## Struttura Repository

```
04-wikipedia-big-data-analysis-pyspark-ml/
├── notebooks/
│   ├── Progetto_EDA.ipynb
│   ├── Progetto_Classificatore (campo Summary).ipynb
│   └── Progetto_Classificatore (campo Documents) .ipynb
└── README.md
```

## Stack Tecnologico

`PySpark 3.x` · `Databricks` · `Spark NLP` · `PySpark MLlib` · `HashingTF` · `LogisticRegression` · `MulticlassClassificationEvaluator`

---

---

# WikiClassifier — Big Data Analysis & Text Classification with PySpark 🇬🇧

![PySpark](https://img.shields.io/badge/PySpark-3.x-E25A1C?logo=apachespark&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-Platform-FF3621?logo=databricks&logoColor=white)

## Overview

Exploratory data analysis and multi-class text classification on Wikipedia articles using Apache Spark on Databricks. Analyses content across 15 knowledge categories, then trains and compares two Logistic Regression classifiers — one on article summaries, one on full article bodies — to determine which text signal better predicts category at distributed scale.

## Dataset

| Attribute | Detail |
|-----------|--------|
| Source | Wikipedia articles (Databricks environment) |
| Fields | `title`, `summary`, `documents`, `categoria` |
| Categories (15) | culture, economics, energy, engineering, finance, humanities, medicine, pets, politics, research, science, sports, technology, trade, transport |

## Pipeline

```
Wikipedia CSV  →  SparkNLP (normalize · lemmatize · stopwords)  →  TF-IDF
    →  Model A (summary) + Model B (documents)  →  MulticlassClassificationEvaluator
```

## Analysis Stages

| Stage | Technique | Output |
|-------|-----------|--------|
| EDA | PySpark + SparkNLP | Per-category stats, word counts, top TF-IDF terms |
| Feature Engineering | CountVectorizer + HashingTF | Distributed feature vectors |
| Classification | Logistic Regression (×2) | Test set accuracy for `summary` vs `documents` |
| Evaluation | MulticlassClassificationEvaluator | Accuracy and F1 comparison between models |

## Setup (Databricks)

```python
!wget https://proai-datasets.s3.eu-west-3.amazonaws.com/wikipedia.csv
import pandas as pd
dataset = pd.read_csv('/databricks/driver/wikipedia.csv')
spark_df = spark.createDataFrame(dataset).drop("Unnamed: 0")
spark_df.write.saveAsTable("wikipedia")
```

Open notebooks in `notebooks/` on a Databricks cluster with SparkNLP installed.

## Technologies

`PySpark 3.x` · `Databricks` · `Spark NLP` · `PySpark MLlib` · `LogisticRegression` · `MulticlassClassificationEvaluator`
