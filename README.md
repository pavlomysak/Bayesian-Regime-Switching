# Bayesian Markov Regime Switching for Equity Volatility

This repository implements a Bayesian two-state Markov regime-switching model to identify latent volatility regimes in daily equity returns. The project focuses on probabilistic regime identification, uncertainty quantification, and model robustness using modern Bayesian tools.

The empirical application studies daily S&P 500 log-returns and estimates persistent low- and high-volatility regimes via a latent Markov process.

---

## Overview

Financial market volatility exhibits clustering and abrupt structural shifts that are not well captured by constant-variance or smoothly evolving models. This project models volatility as a **latent discrete-time regime process**, where each regime corresponds to a distinct variance level and transitions according to a first-order Markov chain.

Key features:
- Explicit modeling of regime persistence and switching behavior
- Full posterior uncertainty over regimes, parameters, and transitions
- Hierarchical prior structure to stabilize inference and enforce identifiability
- Thorough model diagnostics, posterior predictive checks, and prior sensitivity analysis

---

## Model Summary

Let \( Y_t \) denote daily log-returns and \( Z_t \in \{1, 2\} \) the latent volatility regime.

- **Observation model**
  \[
  Y_t \mid Z_t = k \sim \mathcal{N}(0, \sigma_k), \quad k \in \{1,2\}
  \]

- **Latent regime dynamics**
  \[
  Z_t \mid Z_{t-1} \sim \text{Markov}(P), \quad
  P =
  \begin{pmatrix}
  p_{00} & 1 - p_{00} \\
  1 - p_{11} & p_{11}
  \end{pmatrix}
  \]

- **Priors**
  - Hierarchical Gamma priors on regime volatilities to allow partial pooling and asymmetric uncertainty
  - Beta priors on transition probabilities favoring persistent regimes
  - Ordered volatility priors to prevent label-switching

Inference is performed using **MCMC**:
- NUTS for continuous parameters
- Gibbs/Metropolis updates for the latent discrete regime sequence

---

## Data

- Daily closing prices for the S&P 500 index
- Source: Federal Reserve Bank of St. Louis (FRED)
- Prices are transformed into log-returns prior to modeling

---

## Results

The model identifies:
- A **dominant low-volatility regime** with high persistence
- A **less frequent high-volatility regime** characterized by larger variance and greater uncertainty
- Clear temporal clustering of regimes consistent with financial theory

Posterior predictive checks show that the regime-switching specification captures tail behavior of returns substantially better than single-regime Gaussian models. Sensitivity analyses demonstrate that posterior inference is robust to diffuse prior specifications.

---

## Repository Structure

- `notebooks/` – Model implementation and analysis  
- `report/` – Final write-up (LaTeX, PDF)
  - `images/` - Generated images used in report
- `README.md`

---

## Dependencies

- `pymc`
- `arviz`
- `numpy`
- `pandas`
- `matplotlib`
- `scipy`

---

## Reproducibility

All results in the report are fully reproducible using the provided code. Posterior inference may require substantial runtime due to the latent state space and MCMC sampling.

---

## Author

**Pavlo Mysak**  
Boston University  
MA578: Bayesian Statistics  
