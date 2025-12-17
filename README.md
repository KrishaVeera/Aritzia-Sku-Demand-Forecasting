# SKU Demand Forecasting for Inventory Planning
# Overview
This project builds SKU-level demand forecasts to support inventory planning and risk assessment for high-demand retail products.

Using historical daily sales data, the goal is not to predict which products are successful, but to quantify future demand and assess whether upcoming demand could exceed previously handled peak levels.

The project is framed as a pilot forecasting exercise, mirroring how retail teams validate forecasting approaches before scaling across the full catalog.

# Problem Statement
Retail teams must plan inventory for products that already sell well.

The key operational question is:
Will future demand exceed what we have historically been able to handle?

Answering this helps avoid:
- unnecessary overstocking
- reactive replenishment
- stockout risk during peak periods

# Data
Source: Kaggle – Store Item Demand Forecasting

Granularity: Daily sales

Structure: date, store, item, sales

Important note:
The dataset contains only successful SKUs and exhibits strong, calendar-driven seasonality.
This makes it suitable for demand forecasting, not for “product success vs failure” analysis.

# Scope & Design Decisions
Pilot scope: 1 store, 5 SKUs

Why: Full dataset contains 500 time series; pilot scope allows fast iteration and clear interpretation

SKU selection: Chosen based on highest sales variance to maximize forecasting signal

SKU mapping: Items mapped to realistic Aritzia-style product names for storytelling

# Methodology
# 1. ETL
- Filtered to pilot scope
- Renamed SKUs
- Ensured complete daily time series (no missing dates)
- Validated equal history length across all SKUs (1826 days)

# 2. Exploratory Data Analysis (EDA)
EDA confirmed:
- strong, stable annual seasonality
- similar demand shapes across SKUs
- high daily noise → aggregation needed

Key takeaway:
The data is forecastable, even though SKU-level seasonal differences are subtle.

# 3. Forecasting Model
Model: Prophet
Why Prophet:
- Handles long-term seasonality well
- Interpretable trend + uncertainty bands
- Appropriate for decision support rather than pure accuracy optimization
Forecast horizon: 90 days

# 4. Risk Assessment Logic
For each SKU:
Identify last year’s peak daily sales (worst historical demand day)

Compare against:
forecasted peak (expected), forecasted peak (upper confidence bound)

Risk rule:
If forecast upper bound > last year’s peak → elevated risk. Otherwise → low risk

# Results
Across all 5 evaluated SKUs:
Forecasted peak demand remained below last year’s observed peak. Upper confidence bounds also stayed below historical peak levels

# Conclusion:
There is low short-term risk of exceeding previously handled demand levels.

Existing inventory planning assumptions are likely sufficient.

Why This Matters

Forecasting is not about finding surprises - it’s about reducing uncertainty.

This project demonstrates how demand forecasts can:
- validate current inventory strategies
- prevent unnecessary overreaction
- support confident operational decision-making

# Future Work
Integrate inventory-on-hand and lead times to estimate stockout probability

Extend forecast horizon for seasonal buying decisions

Incorporate external demand signals (promotions, trends)

Demand forecasting supports operational confidence, not product discovery.

The value lies in knowing when not to act, backed by data.
