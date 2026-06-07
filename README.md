# Value-at-risk-modelling
4-method VaR backtesting model on SPY (2018–2026). Implements Historical, Parametric, MC GBM &amp; Student-t VaR with Expected Shortfall. Kupiec &amp; Christoffersen backtesting. Key finding: normality assumption rejected — Student-t (df=5) is the only model to pass at 99% 
## Overview
This project implements and backtests four Value-at-Risk (VaR) models on SPY 
(S&P 500 ETF) daily returns from 2018 to 2026 using a rolling 252-day window 
at 99% confidence. The project spans two major market stress periods — the 
COVID-19 crash (2020) and the 2022 Fed rate hike drawdown — providing a 
rigorous real-world test of each model's calibration.

---

## Key Finding
> Normal distribution-based models (Parametric, MC GBM) produce **50 breaches 
> vs 19 expected** at 99% confidence — empirically rejecting the normality 
> assumption for SPY returns. MC Student-t (df=5) is the **only model to pass 
> the Kupiec POF test**, achieving exact calibration with 19 breaches.
> All four models fail the Christoffersen independence test, confirming breach 
> clustering during crisis periods — a known limitation addressable via GARCH extensions.

---

## Models Implemented
| Model | Distribution | Tail Accuracy |
|---|---|---|
| Historical Simulation | Empirical | High |
| Parametric (Variance-Covariance) | Normal | Low |
| Monte Carlo GBM | Normal | Low |
| Monte Carlo Student-t | Student-t (df=5) | High |

---

## Backtesting Results
| Method | Expected | Actual | Kupiec | Christoffersen |
|---|---|---|---|---|
| Historical | 19 | 32 | Fail | Fail |
| Parametric | 19 | 50 | Fail | Fail |
| MC GBM | 19 | 50 | Fail | Fail |
| MC Student-t | 19 | 19 | **Pass** | Fail |

---

## Portfolio Application
$1,000,000 SPY portfolio at 99% confidence:

| Method | Avg Daily VaR | Avg Daily ES |
|---|---|---|
| Historical | $33,682 | $42,685 |
| Parametric | $26,874 | $30,867 |
| MC GBM | $26,931 | $30,928 |
| MC Student-t | $39,135 | $51,935 |

**COVID Peak (March 16, 2020):**
- Historical VaR: $47,924 → ES: $77,286
- Student-t VaR: $48,568 → ES: $63,429
- Parametric VaR: $33,724 → ES: $38,623

---

## Visualisations
- Rolling VaR vs actual SPY returns (all four methods)
- Breach scatter with COVID and 2022 drawdown highlighted
- VaR vs Expected Shortfall gap — Historical & Student-t

---

## Tech Stack
- **Data:** yfinance
- **Computation:** NumPy, Pandas, SciPy
- **Visualisation:** Matplotlib
- **Statistical Tests:** scipy.stats (chi2, norm)

---

## Assumptions
- Log returns stationary within rolling window
- Student-t df=5 fixed, not MLE-calibrated
- Single asset, fully invested, no transaction costs

## Limitations & Extensions
- Breach clustering requires GARCH-filtered Historical Simulation (FHS)
- MLE calibration of Student-t degrees of freedom
- Multi-asset extension via Cholesky decomposition
- Formal ES backtesting via Acerbi-Szekely (2014) out of scope

---
