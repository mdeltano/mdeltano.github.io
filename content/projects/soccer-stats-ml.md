+++
title = "Soccer Player Statistics — ML Goal Prediction"
description = "Supervised regression pipeline predicting soccer player goal totals from a 2,688-row, 124-feature dataset. R² ≈ 0.80 on held-out data."
weight = 4
date = 2023-04-01

[extra]
github = "https://github.com/mdeltano/EECS3401_FinalProject"
# local_image = "soccer-ml.png"
tags = ["python", "tensorflow", "pandas", "numpy", "seaborn", "jupyter"]
+++

## Overview

A supervised machine learning pipeline that uses linear regression
(plus Ridge and Lasso variants) to predict soccer player goal totals
from a public dataset.

## Pipeline

- **Data.** 2,688 rows, 124 features. Full ingestion, cleaning,
  normalization, and feature preparation into model-ready inputs.
- **EDA.** Exploratory data analysis and feature correlation analysis
  using Seaborn to identify statistically meaningful predictors and
  reduce noise.
- **Modeling.** Compared baseline linear regression against Ridge and
  Lasso to assess regularization effects and model stability.
- **Evaluation.** Train/test split with R² as the primary metric.

## Outcome

**~0.80 R²** on held-out data, indicating the chosen feature set
explains roughly 80% of variance in goal totals — strong for a single
linear baseline before tree-based or neural alternatives.

The Jupyter Notebook is structured into clear modular sections for
readability and reproducibility, and the full codebase is on
[GitHub](https://github.com/mdeltano/EECS3401_FinalProject).
