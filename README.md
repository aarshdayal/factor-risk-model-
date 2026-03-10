# Multi‑Factor Equity Risk Model

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
- [Repository Structure](#repository-structure)
- [Installation & Usage](#installation--usage)
- [Future Work](#future-work)
- [License](#license)

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
- **Factor volatilities**: The chart above shows how the volatility of each factor fluctuates over time.
- **Portfolio betas**: The portfolio’s exposures to the factors evolve (see notebook).
- **Risk decomposition**: On average, systematic risk accounts for ~X% of total risk, while idiosyncratic risk makes up ~Y%. This fraction varies over market conditions.

*(Include a few key plots from your notebook in the `reports/` folder and embed them here.)*


