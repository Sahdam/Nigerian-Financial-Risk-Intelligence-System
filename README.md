# Nigerian Financial Risk Intelligence System 🇳🇬

> A complete quantitative financial risk analysis pipeline applied to Nigerian capital markets — combining Cholesky Monte Carlo simulation, Modern Portfolio Theory, and Value at Risk with real NGX equity and Bitcoin data.

---

## Overview

This project builds a **production-grade financial risk intelligence system** for Nigerian market participants. Using historical data for **Guaranty Trust Holding Company (GTCO)**, **Zenith Bank (ZENITHBANK)**, and **Bitcoin (BTC-USD)** over a 7-year period (March 2019 – March 2026), it demonstrates that rigorous quantitative risk analysis is both achievable and necessary in Africa's largest capital market.

The project answers one central question:

> *"If a Nigerian investor put ₦100,000 into each of these assets in March 2019, what is the scientifically measured risk and return profile today — and what is the mathematically optimal way to combine them?"*

---

## Why This Project Matters

### For the Nigerian Retail Investor
Most Nigerians make investment decisions based on tips, sentiment, or raw price history. This project shows what a data-driven approach looks like: measuring exactly how much you could lose on a bad day (VaR), how bad your worst days could be on average (Expected Shortfall), and whether combining assets actually reduces your risk (portfolio optimisation). The answer to that last question is yes — and the numbers prove it.

### For the Nigerian Financial Industry
Nigerian banks and asset managers face growing pressure under Basel III capital adequacy frameworks to implement robust risk measurement systems. This project provides a replicable Python implementation of VaR and Expected Shortfall methodology directly applicable to NGX equity portfolios. The finding that Nigerian equity returns are **leptokurtic** (fat-tailed, kurtosis > 13) has direct implications for capital reserve requirements — normal distribution-based models will systematically underestimate tail risk.

### For Quantitative Analysts and Financial Engineers
The project solves a real-world data engineering problem: applying institutional-grade risk methodology to a frontier market with limited data infrastructure. It includes live data acquisition from the NGX Pulse API, a liquidity screening protocol for NGX stocks, Cholesky factorisation from scratch, and a fully calibrated VaR backtesting framework.

### For Data Scientists
This is a worked example of applying statistical distribution fitting, normality testing, and simulation to a frontier market dataset with specific challenges — including illiquidity (zero-return days), non-normal return distributions, and currency risk from Naira devaluation.

---

## Key Results

| Metric | BTC-USD | ZENITHBANK | GTCO |
|---|---|---|---|
| ₦100,000 grew to | **₦6,889,499** | **₦498,605** | **₦296,000** |
| Annual log return | 42.0% | 23.5% | 15.8% |
| Daily std deviation | 4.08% | 2.43% | 2.30% |
| Annualised Sharpe Ratio | **0.649** | 0.608 | 0.434 |
| Semivariance (downside risk) | 0.001696 | 0.000568 | **0.000482** |
| Kurtosis | >30 | **13.49** | 6.16 |
| Correlation with BTC | — | 0.029 | 0.042 |

### Portfolio Optimisation Results (GTCO + ZENITHBANK)
| Metric | Value |
|---|---|
| Optimal weights | GTCO: 56.45% / ZENITHBANK: 43.55% |
| Effective Number of Bets (ENB) | 1.967 / 2.0 (near-maximum diversification) |
| 95% Value at Risk | 1.436% per ₦100,000 = ₦1,436 |
| 95% Expected Shortfall | 1.780% = ₦1,780 |
| 99% VaR | 1.987% = ₦1,987 |
| ES/VaR Ratio at 99% | 1.159 (moderate tail risk) |
| VaR Backtest Result | 150/3,000 exceedances = exactly 5.0% ✅ |

---

## Project Structure

```
Nigerian-Financial-Risk-Intelligence-System/
│
├── Nigerian_Financial_Risk_Intelligence_System.ipynb   # Main notebook (108 cells)
├── Guaranty Trust Holding Stock Price History.csv      # GTCO historical prices (NGX)
├── Zenith Bank Stock Price History.csv                 # Zenith Bank historical prices (NGX)
└── README.md                                           # This file
```

---

## Analysis Pipeline

The notebook is structured as a complete end-to-end pipeline:

```
Data Acquisition
    ├── NGX Pulse API (live NGX data)
    ├── yfinance (Bitcoin)
    └── GitHub-hosted CSVs (GTCO, ZENITHBANK)
         ↓
Data Engineering
    ├── Outer merge on date index
    ├── Liquidity screening (zero-return filtering)
    └── Log return calculation
         ↓
Exploratory Analysis
    ├── ROI calculation (₦100,000 base)
    ├── Price charts (triple-axis)
    └── Return charts (standardised scale)
         ↓
Volatility Analysis
    ├── High-Low range metric
    ├── 50-day Moving Average deviation
    ├── Standard deviation
    ├── Sharpe Ratio (annualised)
    └── Semivariance (downside risk)
         ↓
Correlation Analysis
    ├── Covariance matrix
    ├── Pearson correlation matrix
    └── Scatter plots with regression lines
         ↓
Distribution Analysis
    ├── Return histograms
    ├── D'Agostino-Pearson normality test
    ├── Jarque-Bera test
    ├── Fat tail analysis (±3σ outlier count)
    ├── Kurtosis measurement
    └── Normal vs t-distribution KDE comparison
         ↓
Monte Carlo Simulation (Cholesky)
    ├── Positive definiteness verification
    ├── Cholesky factorisation
    ├── Correlated sample generation
    ├── Convergence analysis (100 to 20,000 samples)
    └── Visual validation
         ↓
Portfolio Optimisation
    ├── Minimum variance weight calculation
    ├── Efficient frontier visualisation
    ├── Effective Number of Bets (ENB)
    └── Marginal Contribution to Risk (MCR)
         ↓
Value at Risk & Expected Shortfall
    ├── 95% and 99% VaR
    ├── 95% and 99% Expected Shortfall
    ├── VaR backtesting (exceedance counting)
    ├── ES/VaR ratio (tail severity)
    └── Comprehensive 4-panel risk visualisation
```

---

## Installation & Setup

### Requirements

```bash
pip install pandas numpy matplotlib seaborn scipy yfinance requests
```

### Running the Notebook

```bash
git clone https://github.com/Sahdam/Nigerian-Financial-Risk-Intelligence-System.git
cd Nigerian-Financial-Risk-Intelligence-System
jupyter notebook Nigerian_Financial_Risk_Intelligence_System.ipynb
```

The notebook loads NGX data directly from GitHub-hosted CSV files — no local data download is required beyond cloning the repository. Bitcoin data is pulled live from Yahoo Finance via `yfinance`.

---

## Data Sources

| Asset | Source | Format | Period |
|---|---|---|---|
| BTC-USD | Yahoo Finance via `yfinance` | Daily adjusted close | Mar 2019 – Mar 2026 |
| ZENITHBANK | investing.com → GitHub CSV | Daily closing price (₦) | Mar 2019 – Mar 2026 |
| GTCO | investing.com → GitHub CSV | Daily closing price (₦) | Mar 2019 – Mar 2026 |
| Live NGX data | NGX Pulse API | Real-time JSON | Current |

### On Nigerian Equity Data Availability
A key practical challenge documented in this project: `yfinance` does not carry NGX data. Major Nigerian stocks (DANGCEM, MTNN) exhibited severe liquidity problems — 40–60% of trading days showing zero price change — making them unsuitable for statistical analysis. GTCO and ZENITHBANK were selected as the most liquid NGX equities, showing 85%+ non-zero return days over the study period.

---

## Key Technical Implementations

### 1. Cholesky Monte Carlo Simulation
```python
def generate_correlated_stock_samples(n_samples, stock_returns):
    correlation_matrix = stock_returns.corr()
    L = np.linalg.cholesky(correlation_matrix)   # matrix square root
    Z = np.random.standard_normal((2, n_samples)) # independent normals
    X = L @ Z                                     # inject correlation
    return pd.DataFrame(X.T, columns=stock_returns.columns)
```

### 2. Minimum Variance Portfolio Weights
```python
def calculate_minimum_variance_weights(simulated_returns):
    cov = simulated_returns.cov()
    s1, s2, s12 = cov.iloc[0,0], cov.iloc[1,1], cov.iloc[0,1]
    w1 = (s2 - s12) / (s1 + s2 - 2*s12)
    return pd.Series([w1, 1-w1], index=simulated_returns.columns)
```

### 3. Value at Risk Calculation
```python
def calculate_portfolio_var(simulated_returns, weights, confidence_levels):
    portfolio_returns = simulated_returns.values @ weights.values
    var_dict = {}
    for conf in confidence_levels:
        var_dict[conf] = -np.percentile(portfolio_returns, (1-conf)*100)
    return {'portfolio_returns': portfolio_returns,
            'portfolio_mean': np.mean(portfolio_returns),
            'portfolio_std': np.std(portfolio_returns, ddof=1),
            'VaR': var_dict}
```

### 4. Custom InvestmentComparison Function
```python
def InvestmentComparison(startTime, endTime, df):
    # Returns a table of: High-Low, MA Deviation, Std Dev,
    # Daily Return, Sharpe Ratio, Semivariance
    # for any date range and any set of assets
```

---

## Notable Findings

### 1. Near-Zero BTC-NGX Correlation
Bitcoin has a correlation of only 0.029 with Zenith Bank and 0.042 with GTCO. This near-independence means adding Bitcoin to a Nigerian equity portfolio provides genuine diversification — one of the most valuable properties in portfolio construction.

### 2. Fat Tails in Nigerian Equities
Both NGX stocks fail normality tests conclusively (JB p-value = 0.0). ZENITHBANK kurtosis = 13.49 — over four times the normal distribution benchmark of 3.0. The probability of ZENITHBANK's worst observed daily move (-26.15%) under a normal distribution is 9.74 × 10⁻²⁸ — a "never in the universe's lifetime" event that actually occurred. Risk models using normal distributions for Nigerian equities will systematically underestimate tail losses.

### 3. Zenith Bank vs Bitcoin on Risk-Adjusted Returns
Zenith Bank's Sharpe Ratio of 0.608 almost matches Bitcoin's 0.649 — a remarkable finding that a Nigerian banking stock delivered near-equivalent risk-adjusted efficiency to the world's most prominent cryptocurrency over the same period. This is partly explained by Naira devaluation inflating Naira-denominated equity returns.

### 4. Perfect VaR Backtesting
The Monte Carlo VaR model produced exactly 150 exceedances out of 3,000 simulations at 95% confidence — a perfect 5.0% exceedance rate. This validates the Cholesky simulation approach as a well-calibrated risk model.

---

## Limitations

- **Currency risk not isolated**: NGX returns are in Naira; Bitcoin in USD. Naira devaluation (₦359 → ₦1,390 per dollar over 7 years) significantly inflates NGX equity returns in Naira terms.
- **Simulation uses normal base**: The Cholesky simulation draws from standard normals. Given confirmed fat tails, t-distribution sampling would be more accurate.
- **Constant correlation assumed**: The 0.585 GTCO-ZENITH correlation is a 7-year average. Correlations typically spike during market stress events.
- **Transaction costs excluded**: Nigeria's 30% capital gains tax (introduced 2025) is not modelled.
- **Survivorship bias**: Only stocks that remained liquid and listed throughout the study period are included.

---

## Future Work

- [ ] Implement t-distribution base for Cholesky simulation
- [ ] Add rolling VaR backtesting (252-day window)
- [ ] Compute USD-adjusted returns for fair BTC comparison
- [ ] Extend portfolio optimisation to include Bitcoin as a third asset
- [ ] Build a GARCH volatility model to capture time-varying volatility
- [ ] Add Nigerian T-bill (CBN auction data) as a risk-free rate benchmark
- [ ] Deploy as a Streamlit dashboard for non-technical Nigerian investors

---

## Data Ethics Statement

- All data sources are clearly identified and attributed
- Simulated Monte Carlo outputs are explicitly labelled as synthetic — never presented as real trading history
- Survivorship bias is acknowledged: the analysis covers only assets that remained actively traded throughout the study period
- Past performance disclaimer: this project makes no investment recommendations or forward-looking return predictions
- All methodological choices (ddof=1, seed=42, ×252 annualisation) are documented and reproducible

---

## Author

**Sahdam**
MScFE Student | Financial Data Engineering | Nigerian Capital Markets

*This project was developed as part of MScFE 600 Financial Data at WorldQuant University, with the goal of applying institutional-grade risk analytics to the Nigerian financial ecosystem.*

---

## License

This project is licensed under the MIT License. See `LICENSE` for details.

---

## Acknowledgements

- WorldQuant University — MScFE 600 Financial Data curriculum
- NGX Group — Nigerian Exchange Group for market data infrastructure
- Investing.com — Historical NGX equity price data
- Yahoo Finance / yfinance — Bitcoin historical data

---

*"The mathematics of risk is the same whether you are managing a trillion-dollar hedge fund in New York or a ₦100,000 portfolio in Lagos. This project brings those tools to where they are needed most."*
