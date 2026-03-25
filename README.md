Causality in Software Defect Prediction (NASA CM1 Dataset)

This repository implements an end-to-end causal discovery and causal inference pipeline using the NASA CM1 software defect dataset. The goal is to move beyond predictive modeling and uncover causal relationships that explain why software defects occur, enabling more interpretable and actionable insights for engineering systems.

The implementation follows a structured causal workflow: data preprocessing, feature engineering, causal graph discovery, effect estimation, and robustness validation.

📂 Repository Structure

CAUSALITY
src
    cm1.csv
    nasa_kaggle_col_mapping.csv
    Step_1_Causal discovery_NASA dataset.ipynb
    Step_2_Causal inference.ipynb
README.md

Key Components
------------------------------------------------------------------------------------------------------
1. cm1.csv: The NASA CM1 dataset containing software module metrics and defect labels.

2. nasa_kaggle_col_mapping.csv: Mapping file for feature names and descriptions.

3. Step_1_Causal discovery_NASA dataset.ipynb: Implements feature preprocessing and causal structure learning using NOTEARS.
4. Step_2_Causal inference.ipynb: Performs causal effect estimation, refutation, and interpretation using DoWhy.


Methodology Overview
------------------------------------------------------------------------------------------------------
1. Feature Engineering

The dataset includes software metrics such as code size, complexity, and Halstead measures. To reduce multicollinearity and improve interpretability, features are grouped into composite variables:

Size: Combines code-related measures (e.g., LOC, comments)
McCabe: Aggregates control flow complexity metrics
HalStruct: Captures structural diversity (operators/operands)
HalCog: Represents cognitive effort and difficulty

The target variable Defect is treated as a binary indicator.

2. Causal Discovery

We use the NOTEARS algorithm to learn a Directed Acyclic Graph (DAG) from the data. This approach formulates structure learning as a continuous optimization problem with an acyclicity constraint.

Key aspects:

Inputs are standardized continuous features
Domain knowledge is incorporated via constraints (e.g., Size → McCabe allowed, reverse forbidden)
The learned graph reveals causal dependencies among predictors

The resulting structure identifies Size as a key upstream driver, influencing both complexity and defect occurrence.

3. Causal Inference

Using the discovered graph, we estimate the causal effect of Size on Defect:

Treatment: Size
Outcome: Defect
Estimator: Logistic regression (GLM with binomial family)

Since Size is modeled as an upstream variable with no confounders, no adjustment set is required. McCabe is treated as a mediator, not a confounder.

The estimated effect shows that a one standard deviation increase in Size leads to a small but measurable increase in defect probability.

4. Refutation and Robustness

To validate the causal estimate, two refutation methods are applied:

Random Common Cause: Adds synthetic noise variables
Placebo Treatment: Randomizes the treatment variable

High p-values indicate that the estimated effect is stable and not driven by spurious correlations.

5. Intrinsic Causal Influence (ICI)

ICI quantifies how much each feature contributes to the variance in the target:

Size contributes significantly to defect variability
Complexity contributes modestly
Most variance remains unexplained

This highlights the inherent complexity of software defects and limitations of observable metrics.

🚀 Getting Started
------------------------------------------------------------------------------------------------------

Install dependencies:

pip install pandas numpy scikit-learn dowhy notears

Run notebooks in order
------------------------------------------------------------------------------------------------------
Step_1_Causal discovery_NASA dataset.ipynb
Step_2_Causal inference.ipynb

Key Takeaways
------------------------------------------------------------------------------------------------------
Causal methods provide deeper insights than correlation-based models. Size is a primary driver of defects but not the sole factor. Robustness checks are essential for trustworthy causal conclusions. Much of defect variability remains unexplained, reflecting real-world complexity