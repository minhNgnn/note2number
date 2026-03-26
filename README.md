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
- Population: Adult patients (≥18) with Community-Acquired Pneumonia (CAP)
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