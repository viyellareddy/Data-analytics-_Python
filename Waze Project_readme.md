
# Waze Project: User Churn Analysis

## About the Dataset

The dataset used for this project contains user activity data from Waze. The data includes information on user sessions, distances driven, and usage duration. It also indicates whether the user churned or was retained.

Key Fields:
- `label`: Indicates whether the user churned or was retained.
- `device`: Type of device used (iPhone or Android).
- `total_sessions`: Total number of sessions by the user.
- `driven_km_drives`: Total kilometers driven by the user.
- `duration_minutes_drives`: Total duration of drives in minutes.
- `drives`: Number of drives in the last month.
- `driving_days`: Number of days the user drove in the last month.

## Packages Used

The following Python packages are used in this project:

- `pandas`: For data manipulation and analysis.
- `numpy`: For numerical operations.

## Data Analysis

### Initial Data Inspection

We begin by importing the necessary libraries and loading the dataset:

```python
import pandas as pd
import numpy as np

df = pd.read_csv('waze_dataset.csv')
```

### Summary Information

To get an overview of the dataset, the first few rows were inspected and summarize the information:

```python
df.head(10)
df.info()
```

### Handling Null Values

Isolate rows with null values in the `label` column and compare their summary statistics with rows that do not have null values:

```python
null_df = df[df['label'].isnull()]
null_df.describe()

not_null_df = df[~df['label'].isnull()]
not_null_df.describe()
```

### Device Analysis

Check the distribution of device types among rows with and without null values:

```python
null_df['device'].value_counts()
null_df['device'].value_counts(normalize=True)

df['device'].value_counts(normalize=True)
```

### Churn vs. Retained Users

Compare the counts and percentages of churned vs. retained users, and analyze median values for each group:

```python
df['label'].value_counts()
df['label'].value_counts(normalize=True)

df.groupby('label').median(numeric_only=True)
```

Calculate additional metrics such as kilometers per drive and drives per driving day for churned and retained users:

```python
df['km_per_drive'] = df['driven_km_drives'] / df['drives']
median_km_per_drive = df.groupby('label').median(numeric_only=True)[['km_per_drive']]

df['km_per_driving_day'] = df['driven_km_drives'] / df['driving_days']
median_km_per_driving_day = df.groupby('label').median(numeric_only=True)[['km_per_driving_day']]

df['drives_per_driving_day'] = df['drives'] / df['driving_days']
median_drives_per_driving_day = df.groupby('label').median(numeric_only=True)[['drives_per_driving_day']]
```

## Results

### Key Findings

1. Data Types and Missing Values:
   - The dataset contains numeric and non-numeric data types.
   - There are 700 missing values in the `label` column with no obvious pattern.

2. Device Analysis:
   - The distribution of iPhone and Android users is consistent in the overall dataset and among rows with null values.

3. Churn vs. Retained Users:
   - The dataset contains 82% retained users and 18% churned users.
   - Users who churned drove farther and longer in fewer days compared to retained users.
   - Median values show churned users had more drives but used the app less frequently.

4. Additional Metrics:
   - Median kilometers per drive and per driving day are higher for churned users.
   - Median drives per driving day are higher for churned users, indicating intense driving patterns.

## Output

The outputs of our analysis include:

1. Initial Data Inspection:
   - Head of the dataset, data types, and summary statistics.

2. Summary Statistics:
   - Comparison of rows with and without null values.

3. Device Analysis:
   - Distribution and percentage of device types.

4. Churn vs. Retained Users:
   - Counts, percentages, and median values of key metrics.

