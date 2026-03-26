# From Notes to Numbers
### Constructing Disease Severity Scores with NLP-Augmented EHRs

This project develops a framework that integrates text-derived features from clinical notes with structured electronic health records (EHR) using survival analysis and natural language processing (NLP) to improve clinical deterioration prediction.

---

## Overview

Early identification of patient deterioration is critical for timely intervention and improved outcomes. Existing severity scores (e.g., NEWS2) rely primarily on structured data and may overlook valuable information contained in clinical notes.

This project addresses that gap by:
- Extracting semantic information from radiology reports using BioClinicalBERT
- Generating leakage-free text-derived risk scores via GroupKFold
- Integrating these features with structured EHR data
- Applying survival analysis models to predict time-to-deterioration

---

## Methodology

### 1. Cohort Construction
- Data source: MIMIC-IV-ED
- Population: Adult patients (>=18) with Community-Acquired Pneumonia (CAP)
- Outcome: Deterioration within 72 hours:
  - Respiratory failure
  - Circulatory failure
  - In-hospital mortality

### 2. Clinical Text Representation
- Radiology reports encoded using BioClinicalBERT
- 5-fold GroupKFold cross-validation used to:
  - Prevent patient-level data leakage
  - Generate out-of-fold logits (text-derived risk scores)

### 3. Survival Modeling
- Models:
  - Cox Proportional Hazards Model (interpretable)
  - Random Survival Forest (non-linear)
- Features:
  - Structured EHR variables
  - Text-derived risk scores

### 4. Evaluation
- Concordance Index (C-index) for discrimination
- Integrated Brier Score (IBS) for calibration
- 70/30 admission-level split (stratified)
- Bootstrap (N=1000) for confidence intervals

---

## Key Results

- Incorporating text-derived features significantly improves prediction performance over structured data alone
- Models augmented with NLP features achieve:
  - Higher C-index (better ranking)
  - Lower IBS (better calibration)
- Text-derived risk scores enable clear patient risk stratification over time
- Survival models identify text-derived features as the strongest predictors of deterioration

---

## Project Structure

    ├── notebooks/          # Main experiments (run in Google Colab)
    ├── src/                # Data processing and modeling code
    ├── configs/            # Configurations
    ├── pyproject.toml      # Dependency management (uv)
    └── README.md

---

## Setup Instructions

### 1. Data Access via Google BigQuery

This project uses MIMIC-IV-ED hosted on Google BigQuery.

Steps:
1. Obtain access via PhysioNet
2. Create a Google Cloud project
3. Enable BigQuery API
4. Query data:

    SELECT *
    FROM `physionet-data.mimiciv_ed.edstays`
    LIMIT 1000

5. Export data to:
   - Google Drive (recommended for Colab)
   - Or local storage (CSV/Parquet)

---

### 2. Environment Setup (uv)

Install uv:

    pip install uv

Sync environment:

    uv sync

This installs all dependencies defined in `pyproject.toml`.

---

### 3. Running the Project (Google Colab)

Main experiments were conducted in Google Colab.

Steps:
1. Open notebook from `notebooks/`
2. Mount Google Drive:

    from google.colab import drive
    drive.mount('/content/drive')

3. Load dataset from Drive or exported BigQuery data
4. Run all cells sequentially

---

## Notes on Data

Due to data usage restrictions:
- Raw MIMIC data is not included in this repository
- Users must obtain access independently via PhysioNet

---

## Future Work

- Multimodal models combining text, imaging, and structured data
- Time-varying survival models
- External validation across healthcare systems
- Explainability for clinical deployment

---

## Author

Minh Nguyen
MSc Data Science, LMU Munich
Global Connect Fellowship, NTU Singapore
