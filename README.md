# Grand Thera USD/BRL Neural SDE + Regime Colab

Public Google Colab notebook for USD/BRL distributional forecasting and market regime analysis using the Grand Thera / Thera OS API.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/GrandThera/AIT-demo-api/blob/main/api_grandthera_usdbrl_neuralsde_regime.ipynb)

## What This Notebook Shows

- 30-day USD/BRL distributional forecast with the backend Neural SDE model.
- Probability that USD/BRL finishes above a +5% target.
- Probability that USD/BRL touches the +5% target during the horizon.
- Current market regime from the backend Markov regime model.
- Transition matrix, expected dwell time, probability of transition to a USD-strength regime, and probability of leaving the current regime.
- Dashboard plots for transition probabilities, regime-highlighted USD/BRL history, forecast cone, terminal distribution, and empirical comparison.

All forecast and regime model outputs come from the Grand Thera API. The notebook does not run a local HMM, local SDE, local regime model, or local forecast fallback.

## Repository Structure

This repository is intentionally minimal:

```text
.
├── api_grandthera_usdbrl_neuralsde_regime.ipynb
└── README.md
```

## Quick Start

1. Open the notebook using the Colab badge above.
2. Run the setup cell.
3. Run all cells.

The notebook loads a public USD/BRL price series, sends the numeric series to the Grand Thera API, and renders the returned forecast and regime analysis.

## Input Data

The notebook uses public USD/BRL market data as the input series sent to the API.

Primary source:

- Yahoo Finance through `yfinance`, symbol `USDBRL=X`

Backup source for the input series only:

- Banco Central do Brasil SGS 1, USD/BRL commercial selling rate

The BCB path is only a public market-data backup if Yahoo Finance is unavailable. It is not a modeling fallback. Forecasting and regime analysis remain API-generated.

## API Endpoints Used

The notebook calls the public API directly with JSON payloads:

| Goal | Endpoint | Request | Response |
|---|---|---|---|
| Forecast + target probabilities | `POST /api/v1/forecast/run` | `ForecastRunRequest` | `ForecastRunResponse` |
| Regime + transition diagnostics | `POST /api/v1/regime/analyze` | `RegimeAnalyzeRequest` | `RegimeAnalyzeResponse` |

It also checks API availability through:

- `GET /health`

[API documentation](https://api.thera-os.com/docs)

## Notebook Flow

1. Install/import public Python dependencies.
2. Configure the API URL, horizon, target threshold, forecast parameters, and regime parameters.
3. Load the last 252 trading days of USD/BRL prices.
4. Send prices and dates to `/api/v1/regime/analyze`.
5. Send prices, forecast horizon, simulations, and target price levels to `/api/v1/forecast/run`.
6. Normalize API response fields for display.
7. Print a quantitative report.
8. Render dashboard charts.

## Dependencies

The notebook installs or imports:

- `yfinance`
- `requests`
- `scipy`
- `matplotlib`
- `pandas`
- `numpy`

These libraries are used for data retrieval, HTTP requests, response formatting, sanity checks, and visualization. They are not used to replace the backend forecast or regime models.