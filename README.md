<div align="center">

# Heart Disease Exploratory Analysis

**A notebook-based exploration of heart disease data, from first look to a cleaned and scaled feature table.**

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Project stage](https://img.shields.io/badge/Stage-EDA%20%26%20preprocessing-6C63FF)](#project-scope)

</div>

---

## Why this project

This project uses Python to explore a public heart disease dataset, visualize patterns across clinical and demographic features, and prepare the data for a future modeling step. It focuses on making the data easier to inspect and understand before any predictive model is considered.

> **Project scope:** this repository contains exploratory data analysis and preprocessing. It does not train or evaluate a classifier, and its charts are descriptive rather than clinical guidance.

## At a glance

| | |
|---|---|
| **Records** | 918 |
| **Input features** | 11 |
| **Target** | `HeartDisease` (0 = no heart disease, 1 = heart disease) |
| **Target counts** | 410 labeled 0 · 508 labeled 1 |
| **Tools** | Python · pandas · NumPy · Seaborn · Matplotlib · scikit-learn |
| **Main artifact** | `heart_disease_eda.ipynb` |

## Analysis flow

```mermaid
flowchart LR
    A[heart.csv] --> B[Inspect columns and summary statistics]
    B --> C[Explore distributions and target relationships]
    C --> D[Review implausible zero measurements]
    D --> E[Replace zero Cholesterol and RestingBP readings]
    E --> F[One-hot encode categorical features]
    F --> G[Scale selected numeric features]
    G --> H[Prepared data_encode table]
```

The notebook replaces 172 zero Cholesterol readings with the non-zero mean and replaces one zero RestingBP reading with its non-zero mean. It then one-hot encodes categorical columns and scales the selected numeric columns. The original CSV is included so the analysis can be reproduced.

## A look at the data

<table>
  <tr>
    <td align="center"><strong>Target label distribution</strong><br><img src="images/heart_disease_target_distribution.png" alt="Counts of heart disease target labels" width="460"><br><em>508 positive vs 410 negative labels (55% vs 45%); both classes are represented. For a binary target, read the bars and ignore the KDE curve.</em></td>
    <td align="center"><strong>Age distribution</strong><br><img src="images/age_distribution.png" alt="Age distribution in the dataset" width="460"><br><em>Median age is 54; the middle half of records falls between 47 and 60 years.</em></td>
  </tr>
  <tr>
    <td align="center"><strong>Chest pain type by target label</strong><br><img src="images/chest_pain_by_heart_disease.png" alt="Heart disease counts by chest pain type" width="460"><br><em>ASY has 392/496 positive labels (79%); ATA has 149/173 negative labels (86%). These within-category shares describe this dataset, not individual risk.</em></td>
    <td align="center"><strong>Numeric feature correlation heatmap</strong><br><img src="images/numeric_correlation_heatmap.png" alt="Correlation heatmap for numeric features" width="460"><br><em>Oldpeak and MaxHR have the strongest linear correlations with the target (about +0.40 and −0.40); Cholesterol is weak (+0.09). Correlation alone does not show causation or model value.</em></td>
  </tr>
</table>

## Zero-value cleanup: before and after

These paired plots make the data-cleaning step easy to compare: the original distributions with zero readings are on the left, and the distributions after replacing those readings with the non-zero mean are on the right.

### Cholesterol

<table>
  <tr>
    <th>Before cleaning · zero readings present</th>
    <th>After cleaning · zero readings replaced</th>
  </tr>
  <tr>
    <td><img src="images/cholesterol_distribution.png" alt="Cholesterol distribution before replacing zero readings" width="460"></td>
    <td><img src="images/cholesterol_after_replacement.png" alt="Cholesterol distribution after replacing zero readings with the non-zero mean" width="460"></td>
  </tr>
</table>

**What stands out:** 172 of 918 readings (18.7%) are zero, creating a large spike that is more consistent with an unavailable or invalid entry than a measured cholesterol value. After mean replacement, that spike disappears but a tall spike appears around **244.64** because every replaced row receives the same value. This removes zeros while creating an imputation artifact and can understate variability.

### Resting blood pressure

<table>
  <tr>
    <th>Before cleaning · zero reading present</th>
    <th>After cleaning · zero reading replaced</th>
  </tr>
  <tr>
    <td><img src="images/resting_bp_distribution.png" alt="Resting blood pressure distribution before replacing the zero reading" width="460"></td>
    <td><img src="images/resting_bp_after_replacement.png" alt="Resting blood pressure distribution after replacing the zero reading with the non-zero mean" width="460"></td>
  </tr>
</table>

**What stands out:** there is one zero reading in 918 rows. It is replaced with the non-zero mean (**132.54 mmHg**). Since just one record changes, the overall distribution remains nearly the same; the plots use different histogram bins and vertical scales, so compare the overall pattern rather than bar heights.

<details>
<summary><strong>Browse the other 6 notebook charts</strong></summary>

| Visualization | Preview | What it suggests |
|---|---|---|
| Numeric feature distributions | ![Four numeric feature histograms](images/numeric_features_panel.png) | The panel exposes a large zero spike in Cholesterol and a single zero RestingBP reading; Age and MaxHR are broadly centered in their observed ranges. |
| Maximum heart rate distribution | ![Maximum heart rate distribution](images/max_heart_rate_distribution.png) | Median MaxHR is 138 bpm, and the middle half ranges from 120 to 156 bpm. The plot is not split by target label. |
| Sex by target label | ![Heart disease counts by sex](images/sex_by_heart_disease.png) | Positive labels are 458/725 among men (63%) and 50/193 among women (26%) in this sample. The groups differ in size, so raw counts alone are misleading; these are descriptive, unadjusted shares. |
| Fasting blood sugar by target label | ![Heart disease counts by fasting blood sugar](images/fasting_blood_sugar_by_heart_disease.png) | Positive labels are 170/214 (79%) for FastingBS=1 and 338/704 (48%) for FastingBS=0. This is a sample association, not evidence that fasting blood sugar caused the label. |
| Cholesterol by target label | ![Cholesterol distribution grouped by heart disease label](images/cholesterol_by_heart_disease.png) | The median is 244.64 for label 1 and 235 for label 0, but the middle ranges overlap substantially. Replacing many zeros with one mean value also affects this distribution. |
| Age by target label | ![Age distribution grouped by heart disease label](images/age_by_heart_disease.png) | Median age is 57 for label 1 and 51 for label 0. The distributions overlap, so age shows a sample-level association rather than a clean separation. |

</details>

## What the notebook covers

- Initial inspection, summary statistics, and feature types
- Univariate distributions for numeric variables
- Count plots and grouped plots for categorical and numeric variables
- A correlation heatmap for numeric features
- Review and treatment of zero values in Cholesterol and RestingBP
- One-hot encoding of categorical fields
- Standard scaling of selected numeric fields

If model training is added later, the data should be split first and the scaler fitted on training data only (for example, in a scikit-learn pipeline) to avoid leakage.

## Run it locally

You’ll need Python 3.10 or newer.

```bash
git clone https://github.com/AnmolMittal115/heart-disease-exploratory-data-analysis.git
cd heart-disease-exploratory-data-analysis
python -m venv .venv
```

Activate the environment:

```powershell
.venv\Scripts\Activate.ps1
```

On macOS or Linux, use `source .venv/bin/activate`. Then install the dependencies and open the notebook:

```bash
python -m pip install -r requirements.txt
jupyter notebook heart_disease_eda.ipynb
```

Run the notebook cells from top to bottom. The CSV is read from the repository root.

## Repository contents

```text
heart-disease-exploratory-data-analysis/
├── heart_disease_eda.ipynb   # EDA and preprocessing walkthrough
├── heart.csv                 # Source dataset used by the notebook
├── requirements.txt          # Python dependencies
├── images/                   # Exported notebook charts for this README
└── README.md
```

## Dataset

The CSV follows the **Heart Failure Prediction** dataset schema published by [fedesoriano on Kaggle](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction). It includes age, sex, chest pain type, blood pressure, cholesterol, fasting blood sugar, ECG results, maximum heart rate, exercise-induced angina, Oldpeak, ST slope, and the `HeartDisease` target.

