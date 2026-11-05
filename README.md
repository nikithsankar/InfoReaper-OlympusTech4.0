# InfoReaper-OlympusTech4.0
# 🚀 Rebuilding EDEN — Olympus Tech 4.0
**Team:** InfoReaper | **Prefix / Lead Identifier:** InfoReaper  

---

## 📌 Executive Summary
During Olympus Tech 4.0, humanity set out to restore the lost planetary viability evaluation system from the **EDEN archive**. This repository houses the complete, end-to-end data pipeline developed by team **InfoReaper**:
1. **Data Cleaning & Preprocessing:** Systematically handling missing records, filling moon properties with zero, filtering out unrecoverable records, and standardizing data types.
2. **Exploratory Data Analysis (EDA):** In-depth investigation answering key challenge questions (Q1–Q5) and conducting an independent planetary inquiry.
3. **Machine Learning Pipeline (Bonus Round):** A supervised binary classification model built to predict settlement viability (settlement index >= 60) on unseen planetary records.

---

## 🛠️ Data Cleaning & Preprocessing Methodology

Our primary objective in `InfoReaper_notebook_cleaning.ipynb` was to transform raw, incomplete archive records into a pristine dataset (`InfoReaper_cleaned_dataset.csv`) without introducing data bias or artificial noise.

### 1. Handling Missing Data & Filtering
* **Moon-Specific Zero-Fill:** Missing values for moon-specific properties were explicitly filled with zero (0) since absence correctly represents zero presence or non-applicability.
* **Critical Record & Column Filtering:** Rather than arbitrarily imputing core structural identifiers, records or features lacking critical baseline classifications (such as unrecoverable core planetary metadata) were systematically dropped to preserve data integrity and prevent distortion.
* **Numerical Imputation:** For valid continuous features (such as atmospheric oxygen levels, CO2 concentrations, surface pressure, and temperature), medians were applied to skewed distributions and means to normally distributed attributes.

### 2. Standardizing & Type Casting
* Normalized all string fields (stripped leading/trailing spaces, converted inconsistent casing across star/planet classifications).
* Converted numerical fields stored as strings into proper floating-point/integer types.
* Ensured unique identifier integrity (`planetId` / `planetid`).

### 3. Anomaly & Outlier Filtering
* Range checks were executed against physical bounds (e.g., non-negative radiation levels, physically plausible gravity ranges).
* Outliers were flagged using Interquartile Range (IQR) filtering and verified against baseline planetary physics before retention or adjustment.

---

## 📊 Exploratory Data Analysis & Key Findings

In `InfoReaper_notebook_analysis.ipynb` and `InfoReaper_task2_solution.md`, we conducted exhaustive statistical analysis across the cleaned dataset:

* **Questions 1–5:** Evaluated key habitability factors including stellar radiation balance, atmospheric density, liquid water probability, thermal tolerance, and multi-factor resource availability.
* **Independent Investigation:** Analyzed interaction effects between star types and planetary distance to identify high-density viability clusters across system configurations.
* **Visual Artifacts:** Generated full high-resolution plots illustrating feature correlations, distribution spreads, and viability maps.

---

## 🤖 Machine Learning Pipeline (Bonus Round: Rebuilding EDEN)

### 1. Viability Definition
> **Rule:** Planets with a settlement index greater than or equal to 60 (`settlement_index >= 60`) are classified as viable (`True`), while all others are classified as unviable (`False`).

### 2. Feature Engineering & Leakage Prevention
* **Target Isolation:** Stripped `settlement_index`, `planetId`, `planet_name`, and any direct proxy indicators from the training feature matrix to prevent data leakage.
* **One-Hot Encoding:** Applied `pd.get_dummies()` to encode categorical features (`star_type`, `planet_type`).
* **Train/Test Alignment:** Combined feature sets across train and unseen test sets during encoding to guarantee identical column structures (100% dimension alignment).

### 3. Model Architecture & Training
* **Algorithm:** `RandomForestClassifier` (150 estimators, `max_depth=12`, `random_state=42`).
* **Imbalance Mitigation:** Configured `class_weight='balanced'` to treat viable and unviable classes with equal priority.
* **Validation:** Used an 80/20 stratified split (`stratify=y`) for local validation before retraining on 100% of the cleaned training dataset to maximize predictive capacity.

### 4. Predictions Output
Outputs were mapped directly to boolean values (`True` / `False`) and exported to `InfoReaper_predicted.csv` following the exact schema requirement:
```csv
planetId,viable_planet
9,False
10,True
