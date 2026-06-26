# MSc Thesis: Remaining Useful Life Prediction for Lithium-Ion Batteries

This repository contains the implementation and documentation for my Master's thesis on predicting the Remaining Useful Life (RUL) of lithium-ion batteries.

The project focuses on a practical challenge in battery-health modeling: real degradation datasets are often limited, while sequence-based models need enough structured data to learn meaningful aging patterns. To address this, the workflow combines real battery-cycle data with synthetic data generated using SDV's Gaussian Copula Synthesizer, followed by validation, post-processing, feature engineering, and model comparison.

## Project Overview

The goal of this project is to build an end-to-end workflow for lithium-ion battery RUL prediction using:

* real battery-cycle measurements
* synthetic data generation with Gaussian Copula
* statistical validation between real and synthetic data
* domain-specific cleaning and consistency checks
* time-series preparation with a fixed look-back window
* model comparison between LSTM and XGBoost

The work is research-oriented and thesis-based. It is not presented as a production battery-management system.

---

## Problem Context

Remaining Useful Life prediction is an important task in battery health monitoring and predictive maintenance. In practice, battery datasets can be limited, noisy, or collected under narrow operating conditions.

This project addresses two main questions:

* How can synthetic data be used to expand experimentation without ignoring battery degradation constraints?
* How do sequence-aware and tree-based models compare when predicting RUL from battery-cycle data?

---

## Repository Structure

```text
.
├── data/
│   ├── README.md
│   ├── real_battery_cycle_data.csv
│   └── processed_synthetic_battery_data.csv
│
├── docs/
│   └── thesis.pdf
│
├── notebooks/
│   └── RUL_Estimation_and_data_generation.ipynb
│
├── README.md
├── README_thesis.md
├── requirements.txt
└── .gitignore
```

---

## Workflow

The main notebook follows this workflow:

1. Load and inspect the real battery-cycle dataset
2. Assign `DeviceID` values to separate battery/device sequences when RUL resets
3. Generate synthetic data using SDV's Gaussian Copula Synthesizer
4. Apply distribution choices for selected numerical features
5. Filter and clean synthetic data using battery-domain constraints
6. Compare real and synthetic data using correlation analysis and Frobenius norm
7. Select features based on correlation with RUL
8. Scale selected features using MinMax scaling
9. Prepare time-series sequences using a fixed look-back window
10. Train and compare LSTM and XGBoost models
11. Evaluate model performance using regression metrics

---

## Data Strategy

The project uses a real lithium-ion battery-cycle dataset as the basis for preprocessing, synthetic data generation, and validation.

Synthetic data is generated to expand the experimental dataset, but it is not used blindly. After generation, the data is filtered, sorted, and checked against battery-domain constraints, including decreasing RUL behavior over cycle progression.

The `data/` folder includes:

* `real_battery_cycle_data.csv`
* `processed_synthetic_battery_data.csv`

More details are documented in `data/README.md`.

---

## Synthetic Data Validation

The synthetic data is validated by comparing its statistical structure with the real dataset.

Validation steps include:

* comparing real and synthetic correlation matrices
* checking whether key relationships are preserved
* using Frobenius norm to quantify the difference between correlation matrices
* focusing on important relationships such as `Cycle_Index` vs. `RUL`

This validation step is important because the goal is not only to generate more rows, but to generate data that still reflects meaningful battery degradation behavior.

---

## Modeling Approach

Two different modeling approaches are implemented and compared.

### LSTM

The LSTM model is used because battery degradation is sequential. A fixed look-back window is used to transform cycle-level data into a time-series format so that the model can learn from previous cycles.

### XGBoost

XGBoost is used as a strong tabular regression baseline. It is trained on structured features and provides a useful comparison against the sequence-aware LSTM model.

This comparison helps show the trade-off between a deep learning model designed for temporal patterns and a tree-based model designed for tabular data.

---

## Evaluation

The models are evaluated using standard regression metrics:

* RMSE
* MAE
* R²
* Adjusted R²

In the final comparison, the LSTM model showed stronger overall performance, while XGBoost remained a useful baseline for structured tabular prediction.

---

## Key Engineering Decisions

Some important design choices in the project include:

* assigning `DeviceID` values when RUL resets
* generating synthetic data from the real dataset rather than using random augmentation
* applying post-processing after synthetic generation to enforce realistic behavior
* validating synthetic data statistically before modeling
* using a look-back window of 5 cycles for time-series preparation
* comparing two different modeling approaches instead of optimizing only one model

---

## Limitations

This project has clear limitations:

* The workflow is based on one battery-cycle dataset.
* The synthetic data quality depends on the assumptions of the Gaussian Copula model.
* The look-back window is fixed rather than dynamically optimized.
* The project is research-focused and not a deployed real-time prediction system.

These limitations are documented intentionally to keep the scope of the project clear.

---

## How to Run

1. Clone the repository.

2. Install dependencies:

   ```
   pip install -r requirements.txt
   ```

3. Open and run the main notebook:

   ```
   notebooks/RUL_Estimation_and_data_generation.ipynb
   ```

---

## Documentation

For detailed academic discussion, methodology, assumptions, and limitations, see:

* `README_thesis.md`
* `docs/thesis.pdf`

For dataset details, see:

* `data/README.md`

XGBoost also performed robustly but showed weaker performance
in capturing long-term temporal degradation behavior.

---

## Key Engineering Decisions

Key design and engineering decisions include:

- Using synthetic data to scale experimentation while validating statistical consistency
- Enforcing monotonic RUL behavior to reflect physical battery degradation
- Selecting a look-back window of 5 cycles to balance temporal context and noise
- Comparing fundamentally different modeling paradigms rather than optimizing a single model

---

## Reproducibility

The repository structure separates data processing, modeling, and evaluation stages.
Dependencies are explicitly defined, and the workflow can be re-executed
from raw data to final results.

---

## How to Run

1. Clone the repository
2. Install dependencies:

       pip install -r requirements.txt

3. Open and run the notebook:

       notebooks/RUL_Estimation_and_data_generation.ipynb

---

## Academic Documentation

This repository includes the implementation and documentation for my Master's thesis.

For the full academic discussion, methodology, assumptions, and limitations, see:

- `README_thesis.md`
- `docs/thesis.pdf`
