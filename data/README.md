# Data

This folder contains the datasets used in the lithium-ion battery Remaining Useful Life (RUL) prediction workflow.

The project combines real battery-cycle measurements with synthetic data generated using SDV's Gaussian Copula Synthesizer. The synthetic data is not used as a raw replacement for the real dataset; it is filtered and checked to preserve realistic battery degradation behavior before being used in the modeling workflow.

## Files

* `real_battery_cycle_data.csv`
  Real battery-cycle dataset used as the basis for preprocessing, synthetic data generation, and validation.

* `processed_synthetic_battery_data.csv`
  Processed synthetic battery-cycle dataset produced after generation, filtering, sorting, and consistency checks.

  ## Dataset Source

The real battery-cycle dataset is based on the public Kaggle dataset **Battery Remaining Useful Life (RUL)**. The dataset contains lithium-ion battery cycle measurements and RUL labels used for battery remaining-life prediction.

The source should be cited when reusing or extending this project.

## Real Battery Data

The real dataset contains cycle-level lithium-ion battery measurements, including features such as:

* `Cycle_Index`
* `Discharge Time (s)`
* `Decrement 3.6-3.4V (s)`
* voltage-related discharge and charge features
* `Total time (s)`
* `Remaining Useful Life (RUL)`

During preprocessing, a `DeviceID` column is assigned to separate battery/device sequences when RUL resets.

## Synthetic Data

Synthetic data is generated using a Gaussian Copula approach through the SDV library. Specific numerical distributions are assigned to selected features, including beta and uniform distributions for discharge-related timing features.

The notebook generates a large initial synthetic dataset and then applies post-processing steps to keep the data realistic and usable for RUL modeling.

Post-processing includes:

* filtering physically invalid rows
* sorting by `DeviceID`, `Cycle_Index`, and `RUL`
* selecting one row per device-cycle combination
* enforcing decreasing RUL behavior over battery cycles

## Validation

The synthetic data is compared with the real dataset using correlation analysis and Frobenius norm comparison. The goal is to check whether the synthetic data preserves the main statistical relationships found in the original battery-cycle data, especially relationships involving cycle count, discharge behavior, and RUL.

## Note

The processed synthetic dataset in this folder is the cleaned output used for demonstration and reproducibility. The initial synthetic generation step is documented in the main notebook.
