
# Automatidata Project: NYC Taxi Data Analysis

## About the Dataset

The dataset used for this project is the 2017 Yellow Taxi Trip Data provided by the New York City Taxi and Limousine Commission (NYC TLC). It contains detailed information on individual taxi trips, including pickup and dropoff dates/times, passenger count, trip distance, fare amount, and more.

Key Fields:
- `tpep_pickup_datetime`: Timestamp when the trip started.
- `tpep_dropoff_datetime`: Timestamp when the trip ended.
- `passenger_count`: Number of passengers in the taxi.
- `trip_distance`: Distance of the trip in miles.
- `fare_amount`: Cost of the trip.

## Packages Used

- `pandas`: For data manipulation and analysis.
- `numpy`: For numerical operations.
- `jupyter`: For running the Jupyter Notebook.

### Summary Statistics

To get an overview of the dataset, we inspect the first few rows and summarize the information:

```python
df.head(10)
df.info()
df.describe()
```
## Investigating Variables

We sort and interpret the data for two key variables: `trip_distance` and `total_amount`:

```python
df_sort = df.sort_values(by=['trip_distance'], ascending=False)
df_sort.head(10)

total_amount_sorted = df.sort_values(['total_amount'], ascending=False)['total_amount']
total_amount_sorted.head(20)
```

### Additional Analysis

various analyses was performed such as:

- Payment type distribution
- Average tip amount based on payment type
- Mean total amount for each vendor
- Average tip amount for different passenger counts

```python
df['payment_type'].value_counts()

avg_cc_tip = df[df['payment_type'] == 1]['tip_amount'].mean()
avg_cash_tip = df[df['payment_type'] == 2]['tip_amount'].mean()

df.groupby(['VendorID']).mean(numeric_only=True)[['total_amount']]

credit_card = df[df['payment_type'] == 1]
credit_card['passenger_count'].value_counts()

credit_card.groupby(['passenger_count']).mean(numeric_only=True)[['tip_amount']]
```

## Results and key Findings

1. Data Types and Null Values:
   - The dataset contains numeric and non-numeric data types.
   - No null values were found.

2. Variable Distributions:
   - The `fare_amount` variable has a large range with some extreme values, including negative fares which are unusual.
   - The `trip_distance` variable mostly ranges from 1-3 miles, with a few trips significantly longer.

3. Payment Type Analysis:
   - The majority of trips were paid by credit card (type 1) or cash (type 2).
   - The average tip amount for credit card payments is significantly higher than for cash payments.

4. Vendor Analysis:
   - Two vendors are represented in the data, with vendor 2 having slightly more trips than vendor 1.
   - The mean total amount for trips is similar between the two vendors.

5. Passenger Count Analysis:
   - Most trips had one passenger.
   - Average tip amounts vary slightly with the number of passengers but remain relatively consistent.

## Output

The outputs of our analysis include:

1. Initial Data Inspection:
   - Head of the dataset, data types, and summary statistics.

2. Sorted Data:
   - Top 10 trips by distance and top 20 trips by total amount.

3. Descriptive Statistics:
   - Payment type distribution, average tip amounts, and mean total amounts by vendor.

4. Filtered Data:
   - Average tip amounts for different passenger counts (credit card payments only).
