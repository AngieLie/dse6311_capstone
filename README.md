# Predicting Excess Hospital Readmissions from Publicly Reported Quality Data
 
*Working title — a config-driven ML pipeline applied to CMS Hospital Compare data*
 
DSE 6311 Capstone · Preliminary Project Proposal
Team: Vicky Guo + Angeline Setiawan · Merrimack College
 
---
 
## Overview
 
Unplanned hospital readmissions cost Medicare billions annually, and CMS penalizes hospitals with excess readmissions under the Hospital Readmissions Reduction Program (HRRP).
 
This project predicts readmission risk at the **hospital level**, using only **publicly reported quality data**, through a **reusable, config-driven pipeline**.
 
**Research question:** Can publicly reported hospital quality measures, patient-experience (HCAHPS) scores, and safety indicators predict a hospital's excess pneumonia readmission ratio?
 
**Stakeholders:** hospital quality-improvement teams (primary), CMS policy analysts and payers (secondary).
 
---
 
## Hypothesis & Prediction
 
_Hypothesis (the why):_ Publicly reported quality and patient-experience measures reflect underlying care processes — discharge planning, communication, staffing — that influence readmissions. Hospitals scoring worse on these measures should have higher excess readmissions.
 
_Prediction (the result we expect):_ Regression models trained on quality, HCAHPS, and safety features will beat a mean/naive baseline (lower test RMSE, positive R²), with care-process and patient-experience features ranking among the most important.
 
---
 
## Data
 
**Source:** CMS Hospital Compare / Care Compare (`data.cms.gov/provider-data`) — Hospital Readmissions Reduction Program file, HCAHPS patient survey, complications & safety measures. ~4,800+ hospitals, obtained as CSVs, refresh is scripted and repeatable.
 
**Target:** `ExcessReadmissionRatio` (pneumonia cohort, continuous; >1.0 = worse than expected)
 
**Predictors:**
* Clinical quality measures (timely & effective care, mortality, complications)
* HCAHPS patient experience (communication, discharge info, care transitions)
* Safety indicators (hospital-acquired infections, safety grades)
* Structural attributes (ownership type, hospital rating)
 
Any measure derived from readmissions itself is excluded to avoid target leakage.
 
---
 
## Analysis Plan
 
1. **Preprocess** — sentinel/missing handling, imputation, pivot-join sources, encoding, Box-Cox target transform, VIF pruning, scaling, 80/20 split.
2. **Baseline** — regularized linear regression + naive mean-predictor.
3. **Candidates** — LightGBM as primary challenger; pipeline supports up to six model types, logged to MLflow.
4. **Evaluate** — held-out test RMSE and R² with bootstrapped 95% CIs, PCA + k-means exploration, drift checks between runs.
 
**Hypothesis supported if** the best model beats the naive baseline with non-overlapping 95% CIs, and care-process/experience features carry substantial importance.
**Hypothesis challenged if** models don't beat baseline, or predictive power comes only from structural attributes.
 
---
 
## Known Risks
 
* **Hospital-level aggregation** — ecological, not patient-level data; signal may be modest.
* **Target leakage** — some CMS measures are readmission-adjacent; mitigated with an exclusion list.
* **Correlated predictors** — HCAHPS questions are intercorrelated; mitigated with VIF pruning and regularization.
* **Missing/suppressed data** — CMS suppresses small-sample measures; handled via sentinel replacement + imputation.
* **Weak signal** — if R² stays near zero, that's still a valid, reportable finding.
 
---
 
## Tech Stack
 
* **Language:** Python 3.11 (uv-managed), YAML config validated with Pydantic
* **Orchestration:** Apache Airflow 3 (`dag_factory`, one DAG per pipeline)
* **Modeling:** scikit-learn + LightGBM, Pandera validation, ydata-profiling, Evidently drift
* **Tracking & serving:** MLflow registry · FastAPI `/predict` endpoint
* **Infrastructure:** Docker Compose, 8 services (2× Postgres, Airflow, MLflow, FastAPI, nginx)
 
Swapping in a new dataset means adding a new `config/` directory — not new code.
 
---
 
## Team & Rotating Weekly Roles
 
Roles rotate every Monday so both members touch every responsibility. Each person also carries a technical role each week (pipeline/dev + analysis/modeling), swapping by sprint.
 
1. **Team Lead** — sets sprint priorities, runs Monday standup, unblocks work
2. **Documentation Lead** — owns deliverable submissions, keeps docs current
3. **Communication Lead** — owns Discord updates, instructor contact, meeting notes
 
Week 1: Vicky is Team Lead; Angeline covers Documentation and Communication.
 
---
 
## Links
 
* Repo: [github.com/VickyGuo0907/ml_pipeline](https://github.com/VickyGuo0907/ml_pipeline)
* Project board: [github.com/users/VickyGuo0907/projects/1](https://github.com/users/VickyGuo0907/projects/1)

---
# Repository for the DSE6311 Capstone at Merrimack College
## One possible way to consider organizing your Capstone project.

```
capstone-project/
│
├── README.md                  # Project overview and setup instructions
├── LICENSE                    # Optional
├── .gitignore                 # Files Git should ignore
├── requirements.txt           # Dependencies (or environment.yml)
├── FinalDeliverable.pdf
│
├── data/
│   ├── raw/                   # Original data (never modify)
│   ├── interim/               # Temporary cleaned data (do not leave larged datasets)
│   ├── processed/             # Final modeling datasets
│   └── external/              # Public or third-party datasets
│
├── reports/
│	├── proposal/
│   ├── EDA/
│   ├── cleaning_preprocessing/
│   ├── modeling/
│   └── archive/
│
└── source/					   # Source files
    ├── data/				   # Data handling (wrangling, cleaning, preprocessing)
    ├── features/			   # Feature selection
    ├── models/				   # Fitted models
    │	├── baseline/
    │	├── RF/
    │	└── GBM/	
    └── visualization/		   # EDA

```

---
# Student Sign-in
📊  Dr. Geist  🍄
💻 Xin (Vicky) Guo 🎾
💻 Angeline Setiawan 🌸

THIS IS THE LINE FROM THE VICKY BRANCH!
