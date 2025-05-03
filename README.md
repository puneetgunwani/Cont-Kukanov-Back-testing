# Cont-Kukanov-Back-testing
# Smart Order Router Backtest – Cont & Kukanov Static Cost Model

## Overview

This project implements a backtest of a Smart Order Router (SOR) using the static cost model described in *Cont & Kukanov (2013): "Optimal Order Placement in Limit Order Markets"*. The SOR splits a 5,000-share buy order across multiple trading venues using three tunable risk parameters:

- `lambda_over`: penalty for overfilling,
- `lambda_under`: penalty for underfilling,
- `theta_queue`: penalty related to queue position and execution risk.

The goal is to find the parameter set that minimizes total execution cost over a 9-minute mocked market data stream.

---

## Code Structure

- **backtest.py**: Main script containing:
  - `allocate()` — exact implementation of the static allocator (from `allocator_pseudocode.txt`)
  - `simulate_execution()` — replay logic, tracks fills and cash spent
  - `best_ask_strategy()`, `twap_strategy()`, `vwap_strategy()` — benchmark strategies
  - `main()` — runs a grid search on parameter combinations and prints final JSON results

---

## Parameter Search

We tune the following grid:

```python
PARAM_GRID = {
  "lambda_over": [0.1, 1, 10],
  "lambda_under": [0.1, 1, 10],
  "theta_queue": [0.1, 1, 10],
}
