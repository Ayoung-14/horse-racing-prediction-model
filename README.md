# Horse Racing Quant Project

A quantitative analysis of horse racing data: exploratory data analysis, feature engineering, logistic regression modelling with probability calibration, and a value-betting strategy evaluated against Betfair Starting Price (BSP).

---

## Overview

- **Exploratory Data Analysis** — distribution checks, outlier detection, correlation analysis, and data cleaning across 777,549 runner-level observations.
- **Peak Age Analysis** — investigates the relationship between horse age and performance, broken down by race type.
- **Feature Engineering** — constructs chronological cumulative ratings for jockeys and trainers (point-based systems updated per race result), imputes missing official ratings from each horse's historical best, and one-hot encodes high-cardinality categorical IDs (~64,000 sparse features).
- **Predictive Modelling** — trains a regularised logistic regression (L2, grid-searched C) with `RandomUnderSampler` for class imbalance and isotonic calibration via `CalibratedClassifierCV`, achieving AUC 0.789 and near-perfect calibration (ECE 0.001).
- **Value Betting Strategy** — compares calibrated model probabilities to BSP-implied odds, identifies positive-edge bets, and tests profitability with a margin-sensitivity sweep and statistical significance checks.

## Key Techniques

| Area | Methods |
|---|---|
| Data preprocessing | Datetime parsing, categorical encoding, missing-value imputation, outlier analysis |
| Feature engineering | Chronological cumulative ratings (jockeys, trainers), official-rating imputation, one-hot encoding of high-cardinality IDs |
| Modelling | Logistic Regression (L2, grid-searched C), Random Forest (benchmark), `RandomUnderSampler` for class imbalance |
| Calibration | Isotonic regression via `CalibratedClassifierCV`; Brier score, ECE, calibration curves |
| Evaluation | AUC, log loss, coefficient analysis (LR), Gini importance (RF) |
| Betting analysis | BSP-implied probability comparison, flat-stake value betting, margin sensitivity, statistical significance testing |

## Results

| Metric | Before Calibration | After Calibration |
|---|---|---|
| Brier score | 0.195 | 0.082 |
| ECE (10 bins) | 0.294 | 0.001 |

| Model | Test AUC |
|---|---|
| Logistic Regression | 0.789 |
| Random Forest | 0.742 |

- **Model log loss:** 0.282 vs **BSP benchmark:** 0.279

### Betting Evaluation

At a 2% margin threshold, the value-betting strategy placed **16,737 bets** with a flat-stake ROI of **+0.74%**.

However, the 95% confidence interval is **[-0.043, 0.058]**, which includes zero — the positive edge is **not statistically significant**. The margin-sensitivity analysis shows that profitability is fragile and highly threshold-dependent.

## Limitations

- **Not statistically significant.** The observed ROI at the optimal margin threshold does not survive a significance test (95% CI includes zero).
- **No transaction costs.** Commission, slippage, and market impact are not modelled.
- **Single test period.** Results are evaluated on one chronological test split and may not generalise to other time windows.

---

## Repository Structure

```
.
├── alexander_young_horse_racing_quant_test.ipynb   # main analysis notebook (Q0-Q4)
├── requirements.txt                                # pinned Python dependencies
├── datasets/
│   └── data.csv                                    # input race data (~777K rows, not included in repo)
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.10 or later (tested with 3.11)
- The dataset file `datasets/data.csv` must be present locally (not included in the repository due to size)

### Setup

1. **Clone the repository**

   ```bash
   git clone https://github.com/Ayoung-14/horse-racing-prediction-model.git
   cd horse-racing-prediction-model
   ```

2. **Create and activate a virtual environment**

   ```bash
   python -m venv venv
   ```

   Activate it:

   - **macOS / Linux**
     ```bash
     source venv/bin/activate
     ```
   - **Windows (PowerShell)**
     ```powershell
     venv\Scripts\Activate.ps1
     ```
   - **Windows (Command Prompt)**
     ```cmd
     venv\Scripts\activate.bat
     ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Launch the notebook**

   ```bash
   jupyter notebook alexander_young_horse_racing_quant_test.ipynb
   ```
