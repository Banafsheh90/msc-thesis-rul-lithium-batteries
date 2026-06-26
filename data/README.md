# Data

This project uses lithium-ion battery cycle data for Remaining Useful Life (RUL) prediction.

The workflow combines real battery measurements with synthetic data generated using the SDV Gaussian Copula Synthesizer. The synthetic data is used to expand the experimental dataset while preserving key statistical relationships from the original data.

## Dataset Notes

The original dataset includes cycle-level battery features such as:

- `Cycle_Index`
- `Discharge Time (s)`
- `Total time (s)`
- voltage-related discharge features
- `Remaining Useful Life (RUL)`

A `DeviceID` column is assigned during preprocessing to separate battery/device sequences when RUL resets.

## Synthetic Data

Synthetic data is generated using a Gaussian Copula approach. Specific numerical distributions are assigned to selected features, including beta and uniform distributions for discharge-related timing features.

After generation, post-processing is applied to enforce domain consistency, including:

- filtering physically invalid rows
- sorting by `DeviceID`, `Cycle_Index`, and `RUL`
- selecting one row per device-cycle combination
- enforcing decreasing RUL behavior over battery cycles

## Availability

The full dataset is not included directly in this repository by default. This folder documents the data strategy and may include small sample files for demonstration or reproducibility purposes.
