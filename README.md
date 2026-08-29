# End-to-End Credit Risk Modeling & Basel II Compliance Pipeline

An enterprise-grade credit risk analytics system that mirrors the internal quantitative workflows used by retail banks and financial institutions to evaluate credit risk. This project implements a full-stack risk framework to predict, score, and quantify credit exposure across a portfolio of over 800,000 consumer loans using LendingClub data.

---

## 📊 Core Quantitative Framework

This repository calculates the total credit risk profile of a portfolio by modeling the three foundational risk metrics defined under the Basel II international banking accords:

1. **Probability of Default (PD)**: Modeling the statistical likelihood a borrower will fail to meet their credit obligations using Weight of Evidence (WoE) and Logistic Regression.
2. **Loss Given Default (LGD)**: A two-stage machine learning model (Logistic Regression + Linear Regression) predicting the net percentage loss if a borrower defaults.
3. **Exposure at Default (EAD)**: A Linear Regression system forecasting the remaining outstanding dollar balance at the exact moment of default.

The outputs are aggregated to compute the portfolio's total risk footprint:
\[\text{Expected Loss (EL)} = \text{PD} \times \text{LGD} \times \text{EAD}\]

---

## 📁 Repository Map

```text
Credit_Risk_Modeling/
├── data/
│   ├── LCDataDictionary.xlsx       # Tracked feature-to-definition mapping data schema
│   └── loan_data_2007_2014.csv     # Local Raw Source Dataset (~240MB, Git-Ignored)
├── notebooks/
│   ├── Step_1)EDA.ipynb            # Data Ingestion, Profiling, & Missingness Audits
│   ├── Step_2)Pre-Processing.ipynb # Feature cleaning, date-time parsing, text normalization
│   ├── Step_3) Pre-Processing for PD Model.ipynb  # Weight of Evidence (WoE) Variable Binning
│   ├── Step_4) Modeling PD Model.ipynb            # PD Engine & Point-Based Credit Scorecard Development
│   ├── Step_5) Preprocessing for LGD & EAD.ipynb # Recovery state filtering and account structuring
│   ├── Step_6) LGD and EAD Models.ipynb          # Dual-Stage LGD & Linear EAD Model Training
│   └── Step_7) Expected Loss.ipynb               # Portfolio Aggregation & Macro Exposure Audit
├── models/                         # Local Directory for Trained Serialized Binaries (*.sav, Git-Ignored)
├── .gitignore                      # Workspace security guardrails preventing heavy asset leaks
└── requirements.txt                # Fixed pinning of required python libraries
```

---

## 🛠️ Step-by-Step Pipeline Architecture

### Phase 1: Ingestion & Feature Engineering
*   **Step 1: Exploratory Data Analysis**: Missing value strategies, distribution profiling, and identifying credit target variables (`loan_status`).
*   **Step 2: Pre-Processing**: Converting string representations of employment lengths, parsing loan origination dates to compute time-elapsed horizons, and generating dummy encodings.

### Phase 2: Probability of Default (PD) Modeling & Scorecards
*   **Step 3: Fine Classing & Binning**: Grouping continuous parameters into coarse bins. Applying Fine-Classing and Weight of Evidence (WoE) to maximize Information Value (IV) across default horizons.
*   **Step 4: Scorecard Generation**: Fitting a multi-variable Logistic Regression model with strict p-value significance filtering. Coefficients are linearly scaled into an industrial point-based **Credit Scorecard System** mapping directly to credit odds ratios.

### Phase 3: Advanced Risk Components & Capital Allocations
*   **Step 5 & 6: Dual-Stage LGD & EAD Engines**: 
    *   *LGD Stage 1*: A Logistic Regression classifier predicting whether a defaulting borrower will recover any capital at all ($Loss = 100\%$ vs. $Loss < 100\%$).
    *   *LGD Stage 2*: A Linear Regression model predicting the continuous recovery percentage for accounts with non-zero collections.
    *   *EAD Engine*: Continuous Linear Regression modeling the credit conversion factor to project portfolio exposures at default.
*   **Step 7: Expected Loss (EL) Integration**: Multiplying your dynamic PD, LGD, and EAD parameters across current accounts to calculate total bank capital reserve requirements.

---

## ⚙️ Local Build & Installation Instructions

### 1. Recreate the Virtual Workspace Environment
```bash
# Clone this repository locally and step inside
cd Credit_Risk_Modeling

# Build and activate the isolated virtual env
python -m venv venv
source venv/bin/activate  # On Windows PowerShell use: .\venv\Scripts\Activate.ps1

# Install the exact matching mathematical and visualization dependencies
pip install --upgrade pip
pip install -r requirements.txt
```

### 2. Sourcing the Heavy Dataset
Due to GitHub file constraints, the raw data file cannot be tracked online. 
1. Download the uncompressed source `loan_data_2007_2014.csv` file (~240 MB) from your secure alternative mirror or archive backup.
2. Drop that raw file directly into your local **`data/`** directory.

### 3. Execution Pipeline
Boot up your notebook runtime environment and run through the steps sequentially:
```bash
jupyter notebook
```
Executing the notebooks will automatically populate your workspace with local pre-processed files (`loan_data_2007_2014_preprocessed.csv`) and export your trained `.sav` model binary artifacts to the `/models` cache directory.
