# Project 2: Preparing Stock Data for Machine Learning

## Project Overview

This project focuses on preparing historical stock-market data for machine learning. Building on the analysis completed in Project 1, I cleaned the datasets, investigated unusual observations, created new features, combined stock information with historical price data, and prepared the dataset for future predictive modeling.

The main goal of this project is to produce a structured and reliable dataset that can be used as the starting point for **Project 3**, where machine-learning models will be developed and evaluated.

> **Note:** No machine-learning model is built in this project. The focus is entirely on data cleaning, preparation, and feature engineering.

## Datasets

Two datasets from Project 1 were used:

### `historical_stocks.csv`

Contains information about each stock/security, including:

* `ticker`
* `exchange`
* `name`
* `sector`
* `industry`

### `historical_stock_prices.csv`

Contains daily stock-market information, including:

* `ticker`
* `open`
* `close`
* `adj_close`
* `low`
* `high`
* `volume`
* `date`

The two datasets are connected using the `ticker` column.

## Project Objectives

The main objectives of this project were to:

1. Clean and validate the stock data.
2. Investigate unusual price observations.
3. Create useful features for future machine-learning models.
4. Combine stock metadata with historical price data.
5. Encode appropriate categorical variables.
6. Handle missing values created during feature engineering.
7. Perform final data-quality checks.
8. Prepare and save the final dataset for Project 3.

## Data Cleaning

The following cleaning steps were performed:

* Converted the `date` column to datetime format.
* Sorted the price data by `ticker` and `date`.
* Removed the accidental `Symbol` record identified in Project 1.
* Checked the OHLC price fields for basic consistency.
* Reviewed unusually large `close` and `adj_close` values.
* Retained unusual historical observations when they appeared to be valid rather than automatically treating them as errors.
* Represented missing categorical metadata using `UNKNOWN` rather than incorrectly replacing it with numerical values.

### Unusual Observations

Large price observations, particularly those associated with **VIAV around March 2000**, were investigated.

I decided not to automatically remove these observations because they appeared as part of a historical sequence and passed the basic price consistency checks. Therefore, they were retained as unusual historical observations rather than being assumed to be data-entry errors.

## Feature Engineering

Several new features were created from the historical price data.

| Feature           | Description                                           |
| ----------------- | ----------------------------------------------------- |
| `price_change`    | Change in closing price from the previous trading day |
| `daily_return`    | Percentage change in closing price                    |
| `close_lag_1`     | Previous trading day's closing price                  |
| `ma_5`            | 5-day moving average of closing price                 |
| `ma_20`           | 20-day moving average of closing price                |
| `volatility_20`   | 20-day rolling standard deviation of daily returns    |
| `daily_range_pct` | Daily high-low range relative to the closing price    |

I selected `daily_range_pct` as the additional feature because it provides a relative measure of the size of a stock's daily trading range.

## Handling Missing Values

Some missing values were expected after creating lag and rolling features.

For example:

* The first trading day of a stock does not have a previous closing price.
* A 5-day moving average requires at least five observations.
* A 20-day moving average and volatility calculation require sufficient historical observations.

I did not replace these values with zero because zero would introduce artificial information into the dataset.

Instead, rows without the required engineered features were excluded from the modeling-ready dataset.

## Data Integration

The historical price data was merged with the stock metadata using `ticker` as the key.

The following metadata was incorporated:

* `exchange`
* `sector`
* `industry`

The merge was validated to ensure that it did not unexpectedly create duplicate `ticker + date` records.

## Categorical Encoding

`exchange` and `sector` were converted using **one-hot encoding**.

One-hot encoding was selected instead of assigning arbitrary numerical values such as:

```text
NASDAQ = 1
NYSE = 2
```

because assigning numbers would incorrectly imply an order or numerical relationship between the categories.

## Final Dataset

The prepared dataset contains:

* Stock identifiers
* Date
* Open, high, low and close prices
* Adjusted close
* Trading volume
* Price change
* Daily return
* Previous-day close
* 5-day moving average
* 20-day moving average
* 20-day volatility
* Daily high-low range percentage
* Industry information
* One-hot encoded exchange information
* One-hot encoded sector information

The prepared dataset is intended to be used as the starting point for **Project 3**.

## Repository Structure

```text
Project2/
│
├── Project2_Obaje_Paul.ipynb
├── Project2_Obaje_Paul.pdf
├── stock_prepared.csv
├── historical_stocks.csv
├── historical_stock_prices.csv
└── README.md
```

> The original historical price dataset is very large, so the repository may contain the prepared dataset or the processing notebook depending on repository size limitations.

## Tools and Technologies

* Python
* Pandas
* NumPy
* Matplotlib
* Jupyter Notebook
* GitHub

## Key Questions

### What does `daily_return` tell us?

`daily_return` shows the percentage change in a stock's closing price compared with the previous trading day.

### What does a moving average tell us?

A moving average smooths short-term price movements and helps show the recent trend of a stock.

### What does high volatility indicate?

High volatility indicates larger fluctuations in stock returns and greater uncertainty in recent price movements.

### Why do lag and rolling features create missing values?

Lag and rolling features require previous observations. Therefore, the beginning of each stock's history does not contain enough previous data to calculate these features.

### Why is one-hot encoding preferable to assigning numbers to categories?

One-hot encoding prevents categorical values from being given an artificial numerical order.

## Conclusion

This project prepared the historical stock data for future machine-learning analysis. I investigated the main data-quality issues, handled the accidental metadata record, reviewed unusual price observations, engineered new variables, integrated stock metadata, encoded categorical variables, and performed final data checks.

The resulting dataset provides a structured foundation for developing and evaluating predictive machine-learning models in **Project 3**.

---

**Author:** OBAJE PAUL
**Course:** MACHINE LEARNING
**Project:** Project 2 – Data Cleaning and Preparation
