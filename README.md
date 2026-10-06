# Patient-Reported Outcomes Calculator for Total Shoulder Arthroplasty

> **An interactive clinical prediction tool for estimating 1-year patient-reported outcomes after total shoulder arthroplasty (TSA).**

[![R](https://img.shields.io/badge/R-%23276DC3?logo=r\&logoColor=white)](https://www.r-project.org/)
[![Shiny](https://img.shields.io/badge/Shiny-%230088CC?logo=rstudio\&logoColor=white)](https://shiny.posit.co/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-orange)](https://riskcalc.org/Predicting1YearPROMSAfterTotalShoulderArthroplasty/)

## Overview

**Patient-Reported Outcomes Calculator for TSA Patients** is an interactive R/Shiny application that implements a multivariable prognostic model for predicting **1-year PENN Shoulder Score (PSS)** outcomes following primary total shoulder arthroplasty.

The calculator combines preoperative patient, clinical, socioeconomic, disease-specific, and surgical characteristics with the patient's current PSS to generate individualized predictions of:

* **1-year PSS Total**
* **1-year PSS Pain**
* **1-year PSS Function**
* **1-year PSS Satisfaction**

The application is designed to support **preoperative counseling and individualized outcome estimation** for patients undergoing primary TSA.

### 🚀 Try the calculator

**Live application:**
https://riskcalc.org/Predicting1YearPROMSAfterTotalShoulderArthroplasty/

---

## Why this project?

Patient-reported outcomes following shoulder arthroplasty vary substantially across patients.

Rather than reporting only population-level averages, this project translates a multivariable prognostic model into an interactive tool that allows users to explore how an individual's baseline characteristics and current shoulder function relate to expected outcomes at one year.

The application provides a simple interface for entering patient characteristics and either:

1. entering an existing PSS Total score, or
2. completing the integrated PSS questionnaire.

Predictions are then updated dynamically as the patient's information changes.

---

## ✨ Features

### Individualized prediction

The calculator incorporates a broad set of patient- and procedure-level predictors, including:

* Age
* Sex
* Race
* BMI
* Charlson Comorbidity Index
* Smoking status
* Years of education
* Area Deprivation Index (ADI)
* Insurance status
* VR-12 Mental Component Score
* Psychiatric diagnosis
* Opioid use
* Chronic pain
* Prior ipsilateral shoulder surgery
* Diagnosis / implant type
* Glenoid bone loss
* Humeral component fixation
* Superior-posterior rotator cuff repair
* Current PSS Total score

### 🧮 Integrated PSS assessment

Patients who do not already have a PSS Total score can complete the PSS questionnaire directly within the application.

The questionnaire includes pain, satisfaction, and functional activities such as dressing, grooming, reaching, lifting, household activities, sports, and work.

### ⚡ Real-time predictions

The Shiny backend recalculates the predicted outcomes whenever the patient inputs change, allowing users to interactively explore how the predicted 1-year outcome changes with the patient's current profile.

---

## Model

The calculator implements the prognostic models developed in:

> **Sahoo S, Entezari V, Ho JC, et al.**
> *Disease diagnosis and arthroplasty type are strongly associated with short-term postoperative patient-reported outcomes in patients undergoing primary total shoulder arthroplasty.*
> *Journal of Shoulder and Elbow Surgery.* 2024;33(6):e308–e321.
> DOI: **10.1016/j.jse.2024.01.028**

The study analyzed patients undergoing primary shoulder arthroplasty for glenohumeral osteoarthritis (GHOA) or rotator cuff tear arthropathy (CTA), and evaluated patient-, disease-, and surgery-specific factors associated with 1-year postoperative PSS and its subscores. The final analysis included **1,042 patients** with complete data after the study exclusions.

The study found that diagnosis/arthroplasty type, baseline mental health, and other patient and clinical characteristics were important prognostic factors for 1-year postoperative PSS. The authors subsequently developed the online calculator implemented by this repository for individualized preoperative outcome estimation.

---

## Architecture

The application follows a lightweight R/Shiny architecture:

```text
                 ┌─────────────────────┐
                 │     Patient Input    │
                 │                     │
                 │ Demographics        │
                 │ Clinical factors    │
                 │ Surgical factors    │
                 │ Current PSS         │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    Shiny Server     │
                 │                     │
                 │ Input validation    │
                 │ Feature processing  │
                 │ PSS calculation     │
                 │ Model inference     │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Prediction Layer  │
                 │                     │
                 │ 1-year PSS Total    │
                 │ PSS Pain            │
                 │ PSS Function        │
                 │ PSS Satisfaction    │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Interactive Results │
                 └─────────────────────┘
```

### Repository structure

```text
.
├── global.R       # Model loading and prediction functions
├── server.R       # Shiny server logic and dynamic prediction workflow
├── ui.R           # User interface and PSS questionnaire
├── LICENSE        # MIT License
└── README.md
```

### `global.R`

`global.R` loads the trained model objects and defines the prediction functions used by the application.

The prediction layer includes separate functions for:

* PSS Total
* PSS Pain
* PSS Function
* PSS Satisfaction

The PSS Total, Pain, and Function predictions are generated from collections of beta-regression model objects, while the Satisfaction prediction uses its corresponding fitted model object.

### `server.R`

`server.R` handles:

* BMI calculation
* navigation between application pages
* calculation of the current PSS Total
* transformation of user inputs into model-ready features
* model inference
* dynamic rendering of predictions

Predictions are recomputed reactively as the patient's inputs are updated.

### `ui.R`

`ui.R` defines the interactive Shiny interface, including patient characteristics, clinical variables, surgical characteristics, and the embedded PSS questionnaire.

---

## Running locally

This application requires **R** and **Shiny**.

Install the required R packages:

```r
install.packages(c(
  "shiny",
  "shinythemes",
  "plyr",
  "dplyr",
  "ggplot2",
  "reshape2",
  "rms",
  "betareg"
))
```

Then launch the application:

```r
shiny::runApp()
```

The application expects the trained model objects referenced by `global.R` to be available in the application environment.

> **Note:** The clinical dataset used to develop the models is not distributed with this repository.

---

## Clinical context

The underlying study was conducted using data from the Cleveland Clinic Outcomes Management and Evaluation (OME) database.

The original cohort included patients undergoing primary shoulder arthroplasty for:

* Glenohumeral osteoarthritis (GHOA)
* Rotator cuff tear arthropathy (CTA)

The study evaluated anatomic TSA (aTSA) and reverse TSA (rTSA), and investigated associations between baseline patient/disease/surgical characteristics and postoperative patient-reported outcomes.

The published study reported substantial improvement in postoperative PROMs, with **89% of analyzed patients reaching an acceptable symptom state at 1 year**.

---

## ⚠️ Intended use and limitations

This calculator is intended for **research, educational, and clinical decision-support purposes**.

It should not be interpreted as a definitive prediction of an individual patient's future outcome.

Important limitations of the underlying study include:

* The cohort was derived from a single tertiary healthcare system.
* Patients without complete 1-year PROM follow-up were excluded from the primary analysis.
* The analysis was observational.
* The model does not incorporate imaging or objective functional outcomes.
* Generalizability to populations outside the development cohort may be limited.

These limitations are discussed in detail in the original publication.

---

## 📄 Publication

If you use this software, please cite the accompanying publication:

**Sahoo S, Entezari V, Ho JC, et al.**
*Disease diagnosis and arthroplasty type are strongly associated with short-term postoperative patient-reported outcomes in patients undergoing primary total shoulder arthroplasty.*
**Journal of Shoulder and Elbow Surgery.** 2024;33(6):e308–e321.
DOI: 10.1016/j.jse.2024.01.028

The publication describes the development of the prognostic models and the online calculator implemented in this repository.

---

## 🌐 Resources

* **Live calculator:** https://riskcalc.org/Predicting1YearPROMSAfterTotalShoulderArthroplasty/
* **Source code:** https://github.com/jiny-ccf/Predicting1YearPROMSAfterTotalShoulderArthroplasty
* **Publication:** https://doi.org/10.1016/j.jse.2024.01.028

---

## License

This project is released under the **MIT License**. See [`LICENSE`](LICENSE) for details.

---

## Acknowledgments

This work was developed using clinical data from the Cleveland Clinic Outcomes Management and Evaluation (OME) program and reflects a collaboration between clinical investigators, biostatisticians, and software developers.

For questions regarding the calculator or its clinical interpretation, please refer to the original publication and the live application.
