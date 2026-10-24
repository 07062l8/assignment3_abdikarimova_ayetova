# Early Identification of At-Risk University Students

A comparative benchmark of supervised machine learning models for detecting at-risk students early in a course, using the Open University Learning Analytics Dataset (OULAD).

## Overview

This project compares four supervised models (Logistic Regression, Decision Tree, Random Forest and HistGradientBoosting) against a dummy baseline. The goal is to identify students at risk of failing or withdrawing using only data from the first 28 days (4 weeks) of a course module.

- **Authors:** Asima Abdikarimova, Ayazhan Ayetova
- **Group:** SE-2404
- **Dataset:** [OULAD](https://analyse.kmi.open.ac.uk/open-dataset), released under CC BY 4.0

## Repository Structure

```text
.
├── Assignment3_Abdikarimova_Ayetova.ipynb   # Main notebook (Google Colab / Jupyter)
├── requirements.txt                         # Python dependencies
├── .gitignore
└── README.md
```

## Getting Started

1. Clone the repository:

   ```bash
   git clone https://github.com/07062l8/assignment3_abdikarimova_ayetova.git
   cd assignment3_abdikarimova_ayetova
   ```

2. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Download the OULAD CSV files from the [official Open University page](https://analyse.kmi.open.ac.uk/open-dataset) or from [Kaggle](https://www.kaggle.com/datasets/alessandrobotta/open-university-learning-analytics-dataset). Place them in the project root or upload them to Google Colab.

4. Open `Assignment3_Abdikarimova_Ayetova.ipynb` in Google Colab, VS Code or Jupyter Notebook and run all cells.

## Method

- **Prediction time:** only data from the first 28 days of the module is used.
- **Features:** demographic data, early virtual learning environment (VLE) clicks and early assessment scores.
- **Split:** `GroupShuffleSplit` on `id_student`, so no student appears in both the training and test sets (no data leakage).
- **Metrics:** precision, recall, F1-score and ROC-AUC, with the at-risk class as the positive class.

## Results

Evaluation on the held-out test set (5,516 students, 2,433 of them at risk):

| Model                  | Precision | Recall | F1-Score | ROC-AUC |
| ---------------------- | --------- | ------ | -------- | ------- |
| Dummy (baseline)       | 0.000     | 0.000  | 0.000    | 0.500   |
| Logistic Regression    | 0.662     | 0.675  | 0.669    | 0.777   |
| Decision Tree          | 0.662     | 0.647  | 0.655    | 0.760   |
| Random Forest          | 0.690     | 0.665  | 0.677    | 0.791   |
| HistGradientBoosting   | 0.682     | 0.688  | 0.685    | 0.794   |

## Key Findings

- **Best model:** HistGradientBoosting has the highest F1-score (0.685) and ROC-AUC (0.794), followed closely by Random Forest (0.677 and 0.791).
- **Top predictor:** the mean score on early assessments (`mean_score`) is the most influential feature, followed by `code_module` and `highest_education`.
- **Value of early information:** adding early VLE clicks and assessment scores to demographic data raises the F1-score from 0.572 to 0.685.
- **Limitation:** Random Forest missed 815 of the 2,433 at-risk students. The missed students had higher early activity (mean 402.7 clicks versus 146.7), so click counts alone do not separate them reliably.
