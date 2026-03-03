---
title: "AI-Powered Medical Insurance Platform"
date: 2026-03-03
draft: false
tags: ["Machine Learning", "Python", "Predictive Modeling", "Healthcare Analytics"]
summary: "An end-to-end machine learning pipeline optimizing medical insurance claim processing and predicting premium costs."
showToc: true
TocOpen: false
---

## Project Overview
This platform serves as the culminating Capstone project for my AI specialization at Durham College (debuting at the April 2026 Expo). The primary objective is to streamline medical claim processing and accurately forecast insurance premium costs based on patient demographic and health data inputs.

Drawing on my professional experience as a Data Analyst, I prioritized building clean, scalable data pipelines and conducting rigorous exploratory data analysis before training the predictive models.

> 🔒 **Data Privacy & Compliance Notice**
> 
> Due to healthcare data regulations, the raw patient records used to train this model are strictly confidential and cannot be hosted publicly. The code below utilizes a synthetically generated, anonymized dataset that mirrors the exact statistical distributions of the original data to demonstrate the model's architecture and performance safely.

## Data Preprocessing & Pipeline
Before feeding data into the machine learning algorithms, the raw datasets required significant cleaning, normalization, and handling of categorical variables to ensure no data leakage occurred during training.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split

# Load the dataset directly from the hosted portfolio data
data_url = 'https://pranayagarwal7.github.io/data/medical_insurance.csv'
df = pd.read_csv(data_url)

# Isolate numeric columns to prevent type errors during imputation
numeric_cols = df.select_dtypes(include=[np.number]).columns

# Fill missing values explicitly for numeric columns
df[numeric_cols] = df[numeric_cols].fillna(df[numeric_cols].median())

# Encode categorical variables 
df = pd.get_dummies(df, columns=['region', 'smoker'], drop_first=True)

df.head()