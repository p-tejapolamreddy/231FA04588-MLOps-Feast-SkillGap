<img width="818" height="63" alt="image" src="https://github.com/user-attachments/assets/1bdc70d1-a71a-4181-8ebb-b11c9e825009" /># Curriculum-Industry Skill Gap Feature Store Using Feast

## Student Details

**Name:** Tejaswini  
**Register Number:** 231FA04588  
**Section:** 03 

## Project Description

This project develops a Feast-based feature store to identify gaps between curriculum coverage and industry skill demand. It uses feature engineering, historical and online feature retrieval, and a Random Forest model to predict training priorities.

## Problem Statement

The project identifies technical skills where industry demand is higher than curriculum coverage. Feast is used to manage the engineered features consistently for machine-learning training and online prediction.

## Dataset

- **Number of records:** 200
- **Target:** `training_priority`

### Dataset Columns

- `skill_id`
- `skill`
- `curriculum_score`
- `industry_score`
- `curriculum_type`
- `industry_demand`
- `source_type`

### How the Entries Were Created

The dataset was created as a demonstration dataset containing technical skills with curriculum coverage scores and industry demand scores.

## Feature Engineering

| Feature | Meaning |
|---|---|
| `curriculum_score` | Curriculum coverage score |
| `industry_score` | Industry demand score |
| `skill_gap` | Difference between industry and curriculum scores |
| `training_priority` | Indicates training priority |

### Feature Calculation

```text
skill_gap = industry_score - curriculum_score
```

```text
training_priority = 1 if skill_gap >= 20
training_priority = 0 otherwise
```

## Feast Architecture

```text
Original Dataset
      ↓
Feature Engineering
      ↓
Parquet Offline Data
      ↓
Feast FeatureView
      ↓
 ┌─────────────────────┐
 ↓                     ↓
Historical Features   Materialization
 ↓                     ↓
Model Training       Online Store
                       ↓
                  Online Retrieval
                       ↓
                    Prediction
```

## Implementation

### Entity

`skill`

Join key: `skill_id`

### Data Source

`data/skill_gap_features.parquet`

### FeatureView

`skill_gap_features`

Features:

- `curriculum_score`
- `industry_score`
- `skill_gap`
- `training_priority`

### Registration

```bash
feast apply
```

### Historical Feature Retrieval

Historical features were retrieved using Feast.

Output:

`outputs/historical_features.csv`

### Materialization

Feature values were materialized from the offline source into the Feast online store.

### Machine Learning Model

A **Random Forest Classifier** was used.

Input features:

- `curriculum_score`
- `industry_score`
- `skill_gap`

Target:

- `training_priority`

### Online Retrieval

Features were retrieved from the Feast online store and used for model prediction.

Output:

`outputs/online_features.csv`

## Results

### Historical Feature Output

Historical features were successfully retrieved using Feast.

**Output file:** `outputs/historical_features.csv`

### Model Accuracy

**Model Accuracy:** 100.0 %

### Online Feature Output

Online features were successfully retrieved from the Feast online store.

**Output file:** `outputs/online_features.csv`

# Required Analysis

### 1. What is the entity in your Feast implementation?

The entity is `skill`, with `skill_id` as the join key.

### 2. List the features stored in your FeatureView.

- `curriculum_score`
- `industry_score`
- `skill_gap`
- `training_priority`

### 3. Explain how one feature was calculated.

The `skill_gap` feature was calculated using:

```text
skill_gap = industry_score - curriculum_score
```

### 4. What is the difference between your original dataset and the feature dataset?

The original dataset contains raw curriculum and industry information. The feature dataset contains engineered features such as `skill_gap` and `training_priority`, along with timestamps required by Feast.

### 5. What is the purpose of the offline store?

The offline store provides historical feature data for historical retrieval and model training.

### 6. What is the purpose of the online store?

The online store provides feature values for fast retrieval during online prediction.

### 7. What is the purpose of `feast apply`?

`feast apply` registers the Feast entities, data sources, and FeatureViews.

### 8. What does materialization do?

Materialization loads feature values from the offline source into the online store for online retrieval.

### 9. What is the advantage of retrieving features through Feast instead of manually calculating them separately during training and prediction?

Feast provides consistent feature definitions for both training and prediction, reducing differences between training-time and serving-time feature calculations.

### 10. State two limitations of your current dataset.

1. The dataset is a demonstration dataset and does not represent the complete real-world industry job market.
2. Curriculum and industry scores are simplified numerical values.

### 11. State two ways your feature store could be improved when more curriculum and industry evidence becomes available.

1. Integrate real industry job-posting and employer data.
2. Regularly update the feature store with new curriculum and industry evidence.

## Technologies Used

- Python
- Pandas
- Feast
- SQLite
- Scikit-learn
- Random Forest
- Apache Parquet
- Google Colab
- GitHub

## Conclusion

This project demonstrates a complete curriculum-industry skill-gap Feature Store using Feast. The implementation covers feature engineering, Feast entity creation, data source creation, FeatureView creation, registration using `feast apply`, historical feature retrieval, materialization, online feature retrieval, and the use of Feast features in a machine-learning model.
