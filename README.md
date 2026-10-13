# Mega-Cap Index Optimization & Crisis Stress-Testing

![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)
![Data](https://img.shields.io/badge/Data-WRDS%20%2F%20CRSP-orange.svg)
![Domain](https://img.shields.io/badge/Domain-Portfolio%20Optimization%20%26%20Risk-green.svg)

## Overview

In modern equity markets, market-cap-weighted indices (e.g., S&P 500) are increasingly dominated by a small cohort of mega-cap stocks. This concentration raises a fundamental question for asset managers and ETF strategy teams: **Does holding a market-cap-weighted portfolio offer true diversification, or does risk-minimization rebalancing provide superior risk-adjusted returns during macro stress events?**

This project investigates market concentration by evaluating the **15 largest U.S. equities** prior to the 2008 financial crisis. Using parametric Markowitz active-set optimization, we construct a long-only Global Minimum Variance (GMV) portfolio and stress-test its out-of-sample performance against a standard market-cap-weighted benchmark through the 2008–2009 market crash.

---

## Key Findings

* **Structural Disconnect:** The GMV optimization drastically departs from market-cap weighting. The largest constituent (ExxonMobil at 15.0% cap weight) was reduced to **1.2%**, while 5 major mega-caps (including GE, AT&T, Apple, Intel, and Berkshire Hathaway) received **0.0% weight**.
* **Superior Downside Protection:** During the out-of-sample stress period (Jan 2008 – Dec 2009), the frozen GMV portfolio experienced a cumulative loss of **-3.0%** compared to **-15.0%** for the market-cap benchmark—a **5x reduction in drawdown**.
* **Implied Diversification:** Correlation and covariance structures among mega-cap stocks leave substantial room for exploitable risk reduction that market-cap weighting ignores.

---

## Portfolio Comparison & Performance

### In-Sample Estimation (2003-01 to 2007-12)

| Metric | Minimum-Variance Portfolio (Selected) | Market-Cap Weighted Benchmark |
| :--- | :---: | :---: |
| **Expected Monthly Return** | `0.90%` | `1.46%` |
| **Monthly Volatility ($\sigma$)** | **`1.70%`** | `2.43%` |
| **Long-Only Constraint** | Fully Invested ($\sum w_i = 1, w_i \ge 0$) | Fully Invested ($\sum w_i = 1$) |

### Top Stock Weight Allocation Comparison

| Ticker | Company Name | Market-Cap Weight | Minimum-Variance Weight | Difference |
| :--- | :--- | :---: | :---: | :---: |
| **WMT** | Walmart Inc. | 5.6% | **25.1%** | `+19.5%` |
| **BAC** | Bank of America Corp. | 5.4% | **16.5%** | `+11.1%` |
| **JNJ** | Johnson & Johnson | 5.6% | **16.2%** | `+10.6%` |
| **XOM** | Exxon Mobil Corp. | 15.0% | **1.2%** | `-13.8%` |
| **GE** | General Electric Co. | 10.9% | **0.0%** | `-10.9%` |
| **AAPL** | Apple Inc. | 5.1% | **0.0%** | `-5.1%` |

---

## Methodology & Optimization Pipeline

1. **Data Sourcing & Universe Filtering:**
   * **Source:** Historical daily returns pulled from **CRSP (crsp.dsf)** and delisting data (**crsp.dsedelist**) via **WRDS**, linked to Compustat via CCM.
   * **Universe:** Top 15 U.S. common stocks by market capitalization as of December 2007 with complete 60-month historical return series (2003–2007).
   * **Data Cleaning:** Compounded daily returns to monthly observations, merged delisting returns, and filtered non-finite values.

2. **Markowitz Active-Set Optimization:**
   * Implemented the parametric active-set algorithm (**Stein, Branke, and Schmeck, 2008**) to navigate from one corner portfolio to the next along the Efficient Frontier.
   * Enforced strict long-only ($w_i \ge 0$) and fully invested constraints ($\mathbf{1}^T \mathbf{w} = 1$).

3. **Out-of-Sample Crisis Propagation:**
   * Froze optimal target weights at end-of-year 2007 and propagated monthly returns through the 2008–2009 financial crisis to evaluate real-world drawdown and volatility.

---

## Repository Structure

```text
├── data/
│   ├── monthly_stock_clean.parquet       # Compounded monthly stock returns
│   └── monthly_market_cap.parquet        # Market cap data from WRDS
│   └── company_names.csv                 # Company Names
├── notebooks/
│   └── portfolio_optimization.ipynb      # Main execution notebook
├── outputs/
│   ├── cap_vs_mv_weights.png             # Asset allocation bar chart
│   ├── crisis_performance.png            # Out-of-sample growth of $1 plot
│   └── frontier_plot.png                 # Markowitz Efficient Frontier plot
├── README.md                             # Project documentation
└── requirements.txt                      # Python dependencies
```
---

The source code of this project is not publicly released. If you would like to see the source code, please reach me at jessie.deng@uwaterloo.ca.
