# Academic Details – Master’s Thesis

## Research Problem

Accurately estimating the Remaining Useful Life (RUL) of lithium-ion batteries is a key challenge
in battery health management and predictive maintenance.
In practical scenarios, available battery datasets are often limited in size,
collected under narrow operating conditions, and affected by measurement noise.

The core research problem addressed in this thesis is how to build reliable
time-series-based RUL prediction models under data scarcity,
while preserving physically meaningful degradation behavior.
The work focuses not only on predictive accuracy,
but also on the engineering validity of the data and modeling pipeline.

---

## Dataset and Assumptions

The project is based on a real lithium-ion battery dataset containing cycle-level measurements,
including discharge-related timing features, voltage characteristics,
and Remaining Useful Life (RUL) labels.

Several domain assumptions guide the data processing:
- Each battery device follows a monotonic degradation pattern.
- RUL must strictly decrease as the cycle index increases.
- Multiple measurements belonging to the same device and cycle
  should not introduce contradictory RUL values.

These assumptions are explicitly enforced during data preprocessing
to ensure physical plausibility rather than relying solely on model learning.

---

## Synthetic Data Generation Strategy

To address limited real-world data availability,
synthetic data generation is introduced as an explicit design choice
rather than a simple data augmentation step.

Synthetic data is generated using the SDV (Synthetic Data Vault) library
with a Gaussian Copula model.
Feature-wise distributions are selected based on observed real data behavior:
- Beta distributions are used for discharge-related time features
  to reflect bounded, asymmetric characteristics.
- Uniform distributions are applied where appropriate to preserve variability
  without imposing strong parametric assumptions.
- Remaining features follow default Gaussian modeling within the copula framework.

The synthetic dataset is scaled to one million samples
to enable robust experimentation while maintaining statistical structure.

---

## Validation of Synthetic Data

Synthetic data is not assumed to be valid by construction.
Instead, statistical similarity between real and synthetic datasets
is explicitly evaluated.

Validation steps include:
- Comparison of correlation matrices between real and synthetic data.
- Preservation of key relationships, particularly the strong negative correlation
  between cycle index and RUL.
- Quantification of overall correlation differences using the Frobenius norm.

A low Frobenius norm indicates that the synthetic data
captures the dominant dependency structure of the real dataset,
supporting its use for model training and evaluation.

---

## Data Cleaning and Consistency Enforcement

Several post-generation constraints are applied to ensure realistic behavior:
- Removal of physically invalid rows, such as non-positive voltage decrements.
- Sorting data by device identifier, cycle index, and RUL.
- Reduction to a single representative row per device-cycle combination
  to avoid redundant synthetic artifacts.

Additionally, a monotonic RUL enforcement procedure is applied per device,
ensuring that RUL strictly decreases as battery cycles progress.
This step encodes domain knowledge directly into the data pipeline
instead of relying on models to implicitly learn physical constraints.

---

## Feature Engineering Rationale

Feature selection is guided by correlation analysis rather than manual heuristics.
Features with correlation magnitude below a defined threshold
are excluded to reduce noise and redundancy.

Min-Max scaling is applied to continuous features to normalize value ranges
and prevent scale dominance during model training.
Cycle index and RUL are intentionally excluded from scaling
to preserve their interpretability and ordinal structure.

This approach balances statistical rigor with practical model stability.

---

## Time-Series Modeling Design

Battery degradation is inherently sequential.
To capture temporal dependencies, the dataset is transformed
into a supervised time-series format using a fixed look-back window.

A look-back window of five cycles is selected as a trade-off:
- Shorter windows fail to capture sufficient degradation context.
- Longer windows introduce unnecessary noise and increase model complexity.

This design choice reflects a balance between temporal expressiveness
and robustness rather than exhaustive optimization.

---

## Model Selection and Tuning

Two fundamentally different modeling paradigms are implemented and compared:

- **LSTM (Long Short-Term Memory)**  
  Chosen for its ability to model sequential dependencies
  and long-term degradation patterns in time-series data.

- **XGBoost**  
  Used as a strong baseline for tabular regression
  on flattened time-series representations.

Hyperparameter tuning is performed using validation-based searches,
covering learning rates, model capacity, batch size, and training epochs.
This comparative setup highlights the strengths and limitations
of deep sequential models versus tree-based approaches.

---

## Evaluation Metrics and Interpretation

Model performance is evaluated using:
- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)
- Coefficient of Determination (R²)
- Adjusted R²

The LSTM model achieves superior performance,
demonstrating strong capability in capturing time-dependent degradation behavior.
XGBoost shows competitive results on aggregated features
but is less effective in modeling long-term temporal dynamics.

The evaluation emphasizes interpretability and consistency
rather than absolute metric optimization.

---

## Limitations

Several limitations are acknowledged:
- Synthetic data quality is constrained by the statistical assumptions
  of the Gaussian Copula model.
- The look-back window size is fixed rather than dynamically optimized.
- The study focuses on a single battery dataset
  and does not explore cross-dataset generalization.

These limitations define clear boundaries for the current work
and motivate future extensions.

---

## Future Work

Potential directions for future research include:
- Hybrid modeling approaches combining sequence models
  with tree-based predictors.
- Exploration of alternative generative models
  such as variational autoencoders or GANs for time-series data.
- Extension to multi-device transfer learning
  and real-time RUL prediction scenarios.

---

## Thesis Context

This repository represents the complete implementation
and documentation of my Master’s thesis in Computer Science.
The full thesis document and defense presentation
provide additional academic context and formal evaluation.

- thesis/thesis.pdf
- thesis/defense_slides.pdf
