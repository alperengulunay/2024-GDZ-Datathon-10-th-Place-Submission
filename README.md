﻿# District-Based Hybrid Classification + Time-Series Pipeline for Unscheduled Outages

*A winning 10th solution for the GDZ Elektrik Datathon 2024*

---

## Overview / Introduction

Power‑distribution companies face hefty penalties and customer backlash when outages strike unexpectedly.
In this project, I first assessed the daily outage risk score (0–10) for each district using a **Random Forest classifier**.
Then, I used an AutoGluon-based **time series** model to refine the forecast based on that **risk level—predicting** the number of unscheduled outages expected per district per day.
This hybrid approach enables 30-day-ahead forecasts across 47 districts, offering both interpretability and strong predictive accuracy.The final model achieved an **average error of just 1.74 outages** per district per day.


---

## Problem Statement / Objective

* **Business Context:** Outages must be minimized to meet EMRA-imposed KPIs like SAIDI and SAIFI.
* **Data Challenge:** Predict outage duration (`unscheduled_sum`) per district and date, using historical, calendar, and weather-based features.
* **Goal:** Develop an MAE-optimized daily forecasting system to aid in early warnings, operational planning, and customer notification.

---

## Dataset & Features

| File           | Description                         |
| -------------- | ----------------------------------- |
| `train.csv`    | Historical outage records           |
| `test.csv`     | Dates to be predicted               |
| `holidays.csv` | National and regional holidays      |
| `weather.csv`  | Weather metrics per region and time |

**Features used:**

* District (`district`), date (`date`)
* `unscheduled_sum`, `scheduled_sum`
* Derived features from weather (temperature, humidity, cloud cover)
* Calendar signals (holiday flags, day-of-week, etc.)

---

## Solution Architecture

![Solution Architecture](https://github.com/user-attachments/assets/b0de1a69-f649-4a7e-b7bc-908649f9945b)


```text
Raw Data
   │
   ├── Preprocessing per district
   │     ├── Outlier removal
   │     ├── Holiday & weather merge
   │     └── Feature engineering (lags, deltas)
   │
   ├── Modeling (per district)
   │     ├── Classification (outage risk level)
   │     └── Time series regression (MAE-optimized)
   │
   └── Submission formatting
```

All steps are modularized for reproducibility and adaptation to other districts or time horizons.

---

## Modeling Approach

* Each of the 47 districts is modeled **independently** to capture local behavior
* Time series features include lags and temporal deltas
* Calendar and holiday variables are merged with weather data
* Classification models may be used to filter zero-outage days
* Regression models trained on outage magnitudes

> ⚠️ No deep learning or black-box ensembling was used. The pipeline emphasizes clarity and maintainability.

---

## Results & Evaluation


| Model                       | Public MAE | Δ vs. Baseline |
| --------------------------- | ---------: | -------------: |
| Baseline (TS only)          |       1.90 |              – |
| **Hybrid (RF + AutoGluon)** |   **1.74** |         ▼ 0.16 |

> This means the model’s forecasts were off by an average of **1.74 unscheduled outages per district per day**, offering actionable precision for planning crews and alerts.

---


## 🔬  Algorithms & Libraries

| Task           | Library / Model                         | Version |
| -------------- | --------------------------------------- | ------- |
| Classification | `RandomForestClassifier` (scikit‑learn) | 1.4     |
| Time Series    | `AutoGluon TimeSeriesPredictor`         | 0.9     |
| Data Ops       | pandas 2.2 · numpy 1.26                 |         |
| Visuals        | matplotlib 3.8 · seaborn 0.13           |         |


---

## ⚙️ How to Run

```bash
# Install dependencies

# Run notebook
jupyter notebook final-classification-and-time-series.ipynb
```

Note: File paths in the notebook assume local execution. Adjust paths if needed.

## Competition 

```text
Competition Scale:
  - 511 entrants
  - 276 active participants
  - 184 teams
  - 1,987 total submissions
```

🎖️ **Final Rank: Top 10 — Solo submission**


