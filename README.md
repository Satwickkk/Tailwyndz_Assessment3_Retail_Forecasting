# Retail Sales Forecasting & Promotion Effectiveness

## Assessment 3 – Breakfast at the Frat

### Overview

This project analyzes weekly retail sales data to forecast product demand and evaluate the impact of pricing and promotional activities.

The dataset contains weekly transaction-level information across multiple stores and products, along with product attributes, store characteristics, prices, and promotion indicators.

The primary objective is to build a leakage-safe forecasting pipeline that can:

- Forecast weekly product demand
- Compare a simple forecasting baseline with an ML model
- Evaluate price and promotion effects
- Identify important drivers of demand
- Perform an ablation study
- Simulate a pricing scenario
- Provide actionable business recommendations

---

## Business Problem

Retailers need accurate demand forecasts to improve inventory planning, reduce stock-outs, and make better pricing and promotion decisions.

This project addresses the following questions:

1. How accurately can weekly product demand be forecast?
2. Does an ML model outperform a simple moving-average baseline?
3. How important are price and promotional variables?
4. Which factors are the strongest drivers of sales?
5. How does a hypothetical 10% price reduction affect predicted demand?

---

## Dataset

The assessment dataset contains:

- Weekly transaction data
- Store information
- Product information
- Price and base price
- Feature promotion
- Display promotion
- Temporary price reduction (TPR)
- Units sold
- Visits
- Purchasing households
- Store characteristics

The dataset covers weekly observations across multiple stores and products.

The original assessment dataset is not included in this repository.

---

## Data Sources

The workbook contains three main data tables:

### 1. Transaction Data

Contains:

- WEEK_END_DATE
- STORE_NUM
- UPC
- UNITS
- VISITS
- HHS
- SPEND
- PRICE
- BASE_PRICE
- FEATURE
- DISPLAY
- TPR_ONLY

### 2. Store Lookup

Contains store-level attributes such as:

- STORE_ID
- STORE_NAME
- CITY
- STATE
- MARKET AREA
- STORE SEGMENT
- PARKING SPACE
- SALES AREA
- AVERAGE WEEKLY BASKETS

### 3. Product Lookup

Contains:

- UPC
- DESCRIPTION
- MANUFACTURER
- CATEGORY
- SUB_CATEGORY
- PRODUCT_SIZE

---

# Methodology

The project follows the pipeline below:

Data Loading
↓
Data Quality Checks
↓
Data Integration
↓
Exploratory Data Analysis
↓
Price & Promotion Analysis
↓
Feature Engineering
↓
Time-Based Train/Test Split
↓
Moving Average Baseline
↓
CatBoost Forecasting Model
↓
13-Week Holdout Evaluation
↓
Feature Importance
↓
Ablation Study
↓
Price Scenario Analysis
↓
Business Recommendations

---

## Data Preparation

The transaction, product, and store tables were integrated using:

- UPC for product-level mapping
- STORE_NUM / STORE_ID for store-level mapping

Data quality checks included:

- Missing-value analysis
- Duplicate product-store-week checks
- Date validation
- Store and product mapping validation
- Numerical-value validation

---

## Feature Engineering

The forecasting model uses historical demand, pricing, promotions, store attributes, product attributes, and seasonality.

### Historical Demand Features

- 1-week lag
- 2-week lag
- 4-week lag
- 8-week lag
- 13-week lag

### Rolling Features

- 4-week rolling mean
- 8-week rolling mean
- 13-week rolling mean
- 4-week rolling standard deviation

All rolling features are shifted by one period to prevent target leakage.

### Pricing Features

- PRICE
- BASE_PRICE
- DISCOUNT_PCT

### Promotion Features

- FEATURE
- DISPLAY
- TPR_ONLY
- ANY_PROMOTION
- PROMOTION_COUNT

### Calendar Features

- YEAR
- MONTH
- QUARTER
- WEEK_OF_YEAR
- WEEK_SIN
- WEEK_COS

---

# Forecasting Approach

## Baseline

A 4-week moving-average model is used as the baseline.

This provides a simple benchmark against which the machine-learning model can be evaluated.

## Machine Learning Model

CatBoost Regressor is used as the primary forecasting model.

CatBoost was selected because the dataset contains both numerical and categorical variables, including:

- Store
- Product
- Manufacturer
- Category
- Sub-category
- Store segment

---

# Validation Strategy

A time-based validation strategy is used instead of random train/test splitting.

The final 13 weeks are kept as the holdout period.

```text
Historical Data
|-----------------------------|----------|
        Training Data          13-Week
                                Holdout
