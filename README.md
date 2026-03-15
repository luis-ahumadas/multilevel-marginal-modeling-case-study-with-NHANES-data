# Case Study: Multilevel and Marginal Modeling with NHANES Data

This repository contains a comprehensive analysis of statistical dependence in the **National Health and Nutrition Examination Survey (NHANES)** 2015-2016 dataset. The project demonstrates advanced regression techniques—specifically **Generalized Estimating Equations (GEE)** and **Mixed-Effects Models**—to account for geographic clustering in public health data.

---

## Project Overview

In complex surveys like NHANES, data is often collected in clusters (e.g., specific counties or communities) rather than through simple random sampling. This clustering induces statistical dependence: individuals from the same area tend to be more similar to each other than to individuals from different areas.

This notebook explores:

* **Intraclass Correlation (ICC):** Quantifying how much variation in health outcomes (like Systolic Blood Pressure) is due to cluster-level differences.
* **Marginal Models (GEE):** Estimating population-average effects while correcting standard errors for clustering.
* **Multilevel Models (Mixed-Effects):** Estimating cluster-specific random intercepts and random slopes to understand group-level variation.

---

## Key Findings

* **Clustering Impact:** The unadjusted ICC for Systolic Blood Pressure ($BPXSY1$) was approximately **0.028**, indicating that nearly 3% of the variance is attributable to the sampling clusters.
* **Variable Clustering:** Ethnicity ($RIDRETH1$) showed high clustering (ICC ≈ **0.25**), reflecting geographic demographic patterns, while Gender ($RIAGENDR$) showed near-zero clustering.
* **Standard Error Inflation:** Using standard OLS instead of GEE underestimated the standard errors for clustered predictors like Education by over **35%**, potentially leading to false-positive results if not addressed.
* **Random Slopes:** Evidence suggests that the relationship between Age and Blood Pressure varies by cluster, with a positive covariance (0.062) between baseline blood pressure and the rate of increase over time.

---

## Technical Stack

* **Language:** Python
* **Libraries:**
* `pandas` & `numpy`: Data manipulation and cleaning.
* `statsmodels`: Implementation of GEE, GLM, and Mixed Linear Models.
* `seaborn` & `matplotlib`: Statistical visualizations.



---

## Data Dictionary

The analysis focuses on the following variables from the NHANES 2015-2016 cycle:

| Variable | Description |
| --- | --- |
| **BPXSY1** | Systolic Blood Pressure (mmHg) |
| **RIDAGEYR** | Age of participant (years) |
| **BMXBMI** | Body Mass Index ($kg/m^2$) |
| **RIAGENDR** | Gender (1 = Male, 2 = Female) |
| **DMDEDUC2** | Education level (Categorical) |
| **SDMVSTRA** | Pseudo-stratum (Clustering variable) |
| **SDMVPSU** | Primary Sampling Unit (Clustering variable) |

> **Note:** For modeling, "Masked Variance Units" (MVUs) are created by combining `SDMVSTRA` and `SDMVPSU` to ensure unique cluster identifiers across the dataset.

---

## Methodology

### 1. Data Preparation

The dataset is cleaned by removing missing values across the variables of interest to ensure compatibility with `statsmodels` regression packages.

### 2. Marginal Modeling (GEE)

Uses an **Exchangeable** correlation structure to account for the fact that any two individuals in the same cluster share a degree of similarity. This is ideal for estimating "population-level" averages.

### 3. Multilevel Modeling (MixedLM)

Fits **Random Intercepts** and **Random Slopes**.

* **Random Intercepts:** Allows each cluster to have its own baseline blood pressure.
* **Random Slopes:** Uses `re_formula` to allow the effect of Age to vary across different geographic regions.