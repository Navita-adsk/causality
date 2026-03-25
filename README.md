# Causality in Software Defect Prediction (NASA CM1 Dataset)

This repository presents an end-to-end causal analysis pipeline for software defect prediction using the NASA CM1 dataset. Unlike traditional predictive approaches, this project focuses on uncovering **causal relationships** that explain *why* defects occur, enabling more interpretable and actionable insights.

The workflow integrates **causal discovery (NOTEARS)** and **causal inference (DoWhy)** within a structured, reproducible framework.

---
## 📂 Repository Structure

- `src/` – Data and notebooks
  - `cm1.csv` – NASA dataset  
  - `nasa_kaggle_col_mapping.csv` – Feature mapping  
  - `Step_1_causal_discovery_NASA_dataset.ipynb` – Causal graph learning  
  - `Step_2_causal_inference.ipynb` – Effect estimation  
- `README.md` – Project documentation

---

## 🔑 Key Components

- **cm1.csv**  
  NASA CM1 dataset containing software module metrics and defect labels.

- **nasa_kaggle_col_mapping.csv**  
  Mapping file describing feature names and their meanings.

- **Step_1_Causal_discovery_NASA_dataset.ipynb**  
  Data preprocessing and causal graph discovery using NOTEARS.

- **Step_2_Causal_inference.ipynb**  
  Causal effect estimation, validation, and interpretation using DoWhy.

---

## ⚙️ Methodology Overview

### 1. Feature Engineering

The dataset includes software metrics such as code size, complexity, and Halstead measures. To improve interpretability and reduce multicollinearity, features are grouped into composite variables:

- **Size**: Code-related measures (e.g., LOC, comments)  
- **McCabe**: Control flow complexity metrics  
- **HalStruct**: Structural diversity (operators/operands)  
- **HalCog**: Cognitive complexity and effort  

The target variable **Defect** is treated as binary.

---

### 2. Causal Discovery

We apply the **NOTEARS algorithm** to learn a Directed Acyclic Graph (DAG) from the data.

Key aspects:
- Continuous optimization with an acyclicity constraint  
- Standardized input features  
- Domain knowledge incorporated via edge constraints  
  - Example: `Size → McCabe` allowed, reverse forbidden  

The learned graph identifies **Size as a key upstream driver**, influencing both complexity and defect occurrence.

---

### 3. Causal Inference

Using the discovered DAG, we estimate the causal effect of:

- **Treatment**: Size  
- **Outcome**: Defect  
- **Estimator**: Logistic regression (GLM, binomial family)

Under the assumed causal graph, **Size is treated as an upstream variable with no observed confounders**, and thus no adjustment set is used.  
However, this assumption depends on the completeness of observed variables and may not fully capture real-world confounding.

**Result:**  
A one standard deviation increase in Size leads to a **small but measurable increase** in defect probability.

---

### 4. Refutation & Robustness Checks

To validate the causal estimate, we apply:

- **Random Common Cause Test**  
  Introduces synthetic confounders to test stability  

- **Placebo Treatment Test**  
  Randomizes treatment to detect spurious effects  

High p-values indicate that the estimated effect is **robust and not driven by random correlations**.

---

### 5. Intrinsic Causal Influence (ICI)

ICI quantifies each feature’s contribution to outcome variance:

- **Size**: Major contributor  
- **Complexity metrics**: Moderate contribution  
- **Unexplained variance**: Significant  

This highlights the inherent complexity of software defects and limitations of observable metrics.

---

## 🚀 Getting Started

### 1. Install Dependencies

pip install pandas numpy scikit-learn dowhy notears matplotlib networkx

---

### 2. Run the Pipeline

Execute notebooks in order:

1. Step_1_Causal_discovery_NASA_dataset.ipynb  
2. Step_2_Causal_inference.ipynb

---

## 🔁 Reproducibility

- Dataset: NASA CM1 (included)  
- Random seeds fixed where applicable  
- NOTEARS optimization may introduce minor variability  

---

## 📊 Key Takeaways

- Causal methods provide deeper insight than correlation-based models  
- **Size is a primary driver of defects**, but not the sole factor  
- Robustness checks are essential for credible causal conclusions  
- A large portion of defect variability remains unexplained  

---

## ⚠️ Limitations

- Potential **unobserved confounders** (e.g., team practices, developer experience)  
- Results depend on correctness of the learned DAG  
- Findings should be interpreted as **data-driven causal hypotheses**, not definitive truths  

---