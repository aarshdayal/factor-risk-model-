Multi‑Factor Equity Risk Model

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A production‑style implementation of a multi‑factor equity risk model, similar to those used by hedge funds and asset managers. The model decomposes portfolio risk into systematic (factor‑related) and idiosyncratic components using four factors: Market, Size (SMB), Value (HML), and Momentum (MOM).

![Factor Volatility](reports/factor_volatility.png)
*Annualised rolling volatility of the four factors (504‑day window).*

## Table of Contents
- [Overview](#overview)
- [Data Sources](#data-sources)
- [Methodology](#methodology)
- [Results](#results)

## Overview
Portfolio managers need to understand the sources of their risk. This project builds a factor model from scratch using free data. It estimates time‑varying factor exposures (betas) for 50 US stocks, computes rolling factor covariances, and decomposes the total risk of an equal‑weighted portfolio into systematic and specific parts.

## Data Sources
- **Stock prices**: Yahoo Finance (via `yfinance`) – 50 liquid US stocks from the S&P 500.
- **Factor returns**: Kenneth French Data Library (via `pandas_datareader`) – daily Market (Mkt‑RF), SMB, HML, and Momentum factors.
- **Risk‑free rate**: also from French’s library.

All data is publicly available and free.

## Methodology
1. **Data preparation**: Download and align price and factor data.
2. **Rolling regressions**: For each stock, run a 504‑day rolling OLS of excess returns on the four factors to obtain time‑varying betas and residual variances.
3. **Factor covariance**: Using the same rolling window, compute the covariance matrix of factor returns.
4. **Portfolio construction**: Equal‑weighted portfolio of all stocks (rebalanced daily).
5. **Risk decomposition**:
   - Systematic variance = β_p' * Ω * β_p
   - Specific variance = Σ w_i² * σ_ε,i²
   - Total volatility = √(systematic + specific) annualised

## Results
- **Factor volatilities**: The chart in the code shows how the volatility of each factor fluctuates over time.
- **Portfolio betas**: The portfolio’s exposures to the factors evolve (see notebook).
- **Risk decomposition**: On average, systematic risk accounts for ~98.2% of total risk, while idiosyncratic risk makes up ~1.8%. This fraction varies over market conditions.

# Plot 1: Portfolio betas over time
<img width="1385" height="790" alt="image" src="https://github.com/user-attachments/assets/a74531fc-e430-45f0-9fc8-199f27e53f1b" />

# Plot 2: Factor volatility (from covariance diagonal)
<img width="857" height="524" alt="image" src="https://github.com/user-attachments/assets/3f26f4e6-3361-49eb-bca2-9228aa4547d5" />

# Plot 3: Risk decomposition pie chart (average over period)
<img width="515" height="414" alt="image" src="https://github.com/user-attachments/assets/1f5bfc07-3727-4880-88f5-90b00a921516" />

# Plot 4: Evolution of systematic risk fraction
<img width="997" height="447" alt="image" src="https://github.com/user-attachments/assets/33062b40-0ed0-4f2e-957c-5c114cf0df74" />

# Plot 5: Total portfolio annualized volatility over time
<img width="1004" height="447" alt="image" src="https://github.com/user-attachments/assets/bf22aa5c-5c82-4822-8177-6b9e430ff273" />
