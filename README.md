# Survival Analysis & Interactive Patient Survival Calculator

## Heart Attack Patient Survival Prediction Using the WHAS500 Dataset

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Lifelines](https://img.shields.io/badge/Lifelines-Survival%20Analysis-green)](https://lifelines.readthedocs.io/)
[![Gradio](https://img.shields.io/badge/Gradio-Interactive%20App-yellow?logo=gradio&logoColor=white)](https://www.gradio.app/)
[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)

---

## Overview

This project applies survival analysis techniques to the **Worcester Heart Attack Study (WHAS500)** dataset to model and predict patient survival outcomes following acute myocardial infarction (heart attack). It includes a complete analytical pipeline from exploratory data analysis through model evaluation, plus an **interactive Gradio-based calculator** that generates personalized survival predictions from trained models.

> **Disclaimer:** This is an experimental research and portfolio demonstration tool. It is **not** a clinically validated medical device and should not be used for actual clinical decision-making.

---

## Dataset

The **WHAS500** dataset contains records of **500 patients** admitted for heart attack, collected from the Worcester Heart Attack Study. It is a well-known benchmark dataset in survival analysis research.

| Variable | Description | Type |
|----------|-------------|------|
| `age` | Age at hospital admission (years) | Continuous |
| `gender` | Sex (0 = Male, 1 = Female) | Binary |
| `hr` | Initial heart rate (bpm) | Continuous |
| `sysbp` | Initial systolic blood pressure (mmHg) | Continuous |
| `diasbp` | Initial diastolic blood pressure (mmHg) | Continuous |
| `bmi` | Body mass index (kg/m²) | Continuous |
| `cvd` | History of cardiovascular disease (0 = No, 1 = Yes) | Binary |
| `afb` | Atrial fibrillation (0 = No, 1 = Yes) | Binary |
| `sho` | Cardiogenic shock (0 = No, 1 = Yes) | Binary |
| `chf` | Congestive heart failure (0 = No, 1 = Yes) | Binary |
| `av3` | Complete heart block (0 = No, 1 = Yes) | Binary |
| `miord` | MI order (0 = First, 1 = Recurrent) | Binary |
| `mitype` | MI type (0 = Non-Q-wave, 1 = Q-wave) | Binary |
| `los` | Length of hospital stay (days) | Continuous |
| `time` | Follow-up time (days) | Continuous |
| `event` | Vital status (0 = Censored/Alive, 1 = Dead) | Binary |

- **Events observed:** 215 deaths (43.0%)
- **Censored observations:** 285 (57.0%)
- **Follow-up range:** 1 to 2,358 days (median: 632 days)

---

## Project Structure

```
Survival-Analysis-Healthcare-Analytics/
│
├── WHAS500_Survival_Analysis.ipynb    # Complete survival analysis notebook
├── WHAS500_Survival_Calculator.ipynb   # Interactive patient survival calculator
├── whas500.csv                         # Dataset
├── cox_model.pkl                       # Trained Cox PH model
├── aft_model.pkl                       # Trained Weibull AFT model
├── feature_list.pkl                    # Feature list used by models
└── README.md                           # Project documentation
```

---

## Part 1: Survival Analysis Notebook

**`WHAS500_Survival_Analysis.ipynb`**

A comprehensive survival analysis covering the full analytical pipeline:

### Exploratory Data Analysis
- Dataset inspection, summary statistics, and missing value assessment
- Distribution analysis for continuous and categorical variables
- Correlation analysis and multicollinearity checks
- Visualization of survival patterns across demographic and clinical subgroups

### Kaplan-Meier Estimation
- Overall survival curve with confidence intervals
- Stratified survival curves by gender, congestive heart failure, cardiogenic shock, MI order, atrial fibrillation, and cardiovascular disease
- Log-rank tests for statistical significance of group differences
- Median survival time estimation

### Cox Proportional Hazards Model
- Semi-parametric regression with hazard ratio interpretation
- Proportional hazards assumption testing (Schoenfeld residuals)
- Concordance index evaluation
- Influential covariate identification

### Accelerated Failure Time (AFT) Models
- Weibull, Log-Normal, and Log-Logistic distributions
- AIC-based model comparison
- Acceleration factor interpretation
- Best AFT distribution selection

### Random Survival Forest (IPCW-based)
- Ensemble-based survival modeling using Inverse Probability of Censoring Weighting
- Implementation without scikit-survival dependency
- Hyperparameter tuning and variable importance ranking

### Model Comparison
- 5-fold cross-validated concordance index for all models
- Integrated Brier Score (IBS) for calibration assessment
- Comprehensive performance comparison table
- Final model recommendation with justification

---

## Part 2: Interactive Patient Survival Calculator

**`WHAS500_Survival_Calculator.ipynb`**

A Gradio-based web application that generates real-time, patient-specific survival predictions.

### Features

**Patient Input Form**
- 13 clinical variables collected at hospital admission
- Appropriate input controls: numeric sliders, dropdowns, and binary choices
- Input validation with clinically meaningful range checks
- `los` (length of stay) excluded to prevent data leakage — it is unknown at the time of admission

**Prediction Engine**
- Dual-model predictions using Cox PH (primary) and Weibull AFT (secondary)
- Survival probabilities at 5 time horizons: 30 days, 90 days, 1 year, 3 years, and 5 years
- Corresponding mortality (cumulative event) probabilities
- Median predicted survival time
- Relative risk score compared to the average patient

**Results Dashboard**
- Color-coded survival probability cards (green/orange/red/purple)
- Interactive Plotly survival curve with tooltips and time horizon markers
- Patient input summary and data validation status
- Clinical interpretation with appropriate limitations

**Validation Suite**
- Automated testing with low-risk, high-risk, and average patient profiles
- Invalid input rejection tests (out-of-range values, impossible blood pressure combinations)
- Probability consistency checks (values between 0 and 1, non-increasing over time)

### Calculator Screenshots

After launching, the calculator provides:
1. A clean input form for entering patient clinical data
2. A professional dashboard displaying survival probability cards
3. An interactive survival curve chart
4. A clinical interpretation section

---

## Models

### Cox Proportional Hazards (Primary)
- **Cross-validated C-index:** 0.7667 ± 0.0355
- **In-sample C-index:** 0.7843
- Semi-parametric model; makes no assumption about the baseline hazard
- Outputs full survival curves via `predict_survival_function()`
- Provides hazard ratios and partial hazard (risk) scores

### Weibull AFT (Secondary)
- **Cross-validated C-index:** 0.7669 ± 0.0378
- **In-sample C-index:** 0.7848
- Parametric model assuming Weibull-distributed survival times
- Outputs full survival curves and median survival time
- Provides acceleration factors for covariate interpretation

### Random Survival Forest (Analysis Only)
- **Cross-validated C-index:** 0.7099 ± 0.0116
- **In-sample C-index:** 0.7732
- IPCW-based implementation (does not require scikit-survival)
- Used for model comparison and variable importance — not included in the calculator because the IPCW implementation does not support survival curve generation

---

## Installation

### Prerequisites
- Python 3.10 or higher
- Jupyter Notebook or JupyterLab

### Install Dependencies

```bash
pip install pandas numpy scipy matplotlib seaborn lifelines scikit-learn joblib gradio plotly
```

### Optional (for full Random Survival Forest support)

```bash
pip install scikit-survival
```

> **Note:** scikit-survival requires C++ build tools on Windows. Install [Visual Studio Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) and select "Desktop development with C++" if the installation fails.

---

## Usage

### Running the Analysis Notebook

```bash
jupyter notebook WHAS500_Survival_Analysis.ipynb
```

Run all cells sequentially (Kernel → Restart & Run All).

### Running the Calculator

```bash
jupyter notebook WHAS500_Survival_Calculator.ipynb
```

1. Run all cells — the final cell launches the Gradio web interface
2. Open `http://127.0.0.1:7860` in your browser
3. Enter patient clinical data and click **Predict**
4. View survival probabilities, the interactive survival curve, and clinical interpretation

> The trained model files (`cox_model.pkl`, `aft_model.pkl`, `feature_list.pkl`) must be in the same directory as the calculator notebook, or in a `models/` subfolder. The notebook will train and save them automatically on first run if they are not found.

---

## Key Results

| Metric | Cox PH | Weibull AFT | RSF (IPCW) |
|--------|--------|-------------|------------|
| CV C-index | 0.7667 ± 0.0355 | 0.7669 ± 0.0378 | 0.7099 ± 0.0116 |
| In-sample C-index | 0.7843 | 0.7848 | 0.7732 |
| Survival curves | Yes | Yes | No |
| Median survival | Yes | Yes | No |
| Risk scores | Yes | Yes | Yes |

Both Cox PH and Weibull AFT achieved strong discrimination (C-index > 0.76) and support full survival curve prediction. The Weibull AFT model had a marginally higher cross-validated C-index and was selected as the secondary model for the calculator.

---

## Limitations

- **Single-center dataset:** WHAS500 comes from Worcester, Massachusetts. Results may not generalize to other populations, geographies, or healthcare systems.
- **Sample size:** 500 patients limits the precision of estimates, especially for rare subgroups.
- **Historical data:** Treatment protocols, medications, and clinical practices may have changed since data collection.
- **No external validation:** Models have been evaluated using cross-validation on the same dataset but have not been tested on an independent cohort.
- **Calibration:** Cross-validated concordance index measures discrimination (ranking patients by risk), not calibration (accuracy of predicted probabilities). Predictions should be interpreted as relative risk estimates.
- **Missing predictors:** The models use only the variables available in WHAS500. Important clinical factors such as troponin levels, ejection fraction, medication history, and comorbidity indices are not included.
- **Not a clinical tool:** This calculator is a research demonstration. It has not undergone regulatory review and must not be used for patient care decisions.

---

## Technologies

- **Python** — Core programming language
- **pandas / NumPy / SciPy** — Data manipulation and computation
- **lifelines** — Survival analysis (Kaplan-Meier, Cox PH, Weibull AFT)
- **scikit-learn** — Preprocessing, cross-validation, and Random Forest
- **matplotlib / seaborn** — Static visualizations
- **Plotly** — Interactive survival curves
- **Gradio** — Web-based interactive calculator interface
- **joblib** — Model persistence and serialization

---

## Author

**Arjohn Calig-onan**

Data Operations Specialist | Data Analyst & Statistician

[![GitHub](https://img.shields.io/badge/GitHub-JohnAnalytics0315-181717?logo=github)](https://github.com/JohnAnalytics0315)

---

## License

This project is available for educational and research purposes. See [LICENSE](LICENSE) for details.
