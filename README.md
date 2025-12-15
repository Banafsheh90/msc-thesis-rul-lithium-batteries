# MSc Thesis: Remaining Useful Life Prediction for Lithium-Ion Batteries

This repository contains the complete implementation of my Master’s thesis in Computer Science,
focused on building an end-to-end engineering pipeline for predicting the Remaining Useful Life (RUL)
of lithium-ion batteries.

The work goes beyond model training and addresses practical challenges such as limited real-world data,
synthetic data validation, physically consistent degradation behavior,
and trade-offs between different modeling approaches.

---

## Problem Context

Accurate estimation of Remaining Useful Life (RUL) is a critical problem in battery health monitoring,
predictive maintenance, and energy storage systems.
In real-world settings, battery datasets are often small, noisy,
and collected under limited operating conditions.

From an engineering perspective, this raises two key challenges:
- How to build reliable prediction models with limited real data
- How to scale experimentation without violating physical degradation constraints

---

## Solution Overview

The implemented system follows a structured, end-to-end pipeline:

1. Data loading and preprocessing
2. Device-level segmentation and validation
3. Synthetic data generation using Gaussian Copula (SDV)
4. Statistical validation of synthetic data
5. Data cleaning and consistency enforcement
6. Feature engineering and scaling
7. Time-series preparation using a fixed look-back window
8. Model training and hyperparameter tuning
9. Quantitative evaluation and model comparison

---

## Data Strategy

The project combines real lithium-ion battery data with a large synthetic dataset
generated using the SDV library and a Gaussian Copula model.

Synthetic data is not used blindly.
Its statistical consistency with the real dataset is explicitly validated
using correlation analysis and Frobenius norm comparison.
Additionally, domain constraints such as monotonic RUL degradation
are enforced to preserve physical realism.

---

## Models

Two different modeling approaches are implemented and compared:

- **LSTM (Long Short-Term Memory)**  
  Designed for sequential time-series data and capable of capturing long-term degradation patterns.

- **XGBoost**  
  A strong baseline for tabular regression trained on flattened time-series features.

This comparison highlights the trade-offs between sequence-aware deep learning models
and efficient tree-based approaches.

---

## Results

The LSTM model achieved the strongest overall performance:

- RMSE: 46.54  
- MAE: 12.91  
- R²: 0.979  

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
3. Run the notebooks in order to reproduce the results


---

## Academic Documentation

This repository represents my complete Master’s thesis work.

For detailed academic discussion, methodology, assumptions, and limitations, see:
- `README_thesis.md`
- `thesis/thesis.pdf`
- `thesis/defense_slides.pdf`
