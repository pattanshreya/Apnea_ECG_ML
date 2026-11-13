# Sleep Apnea Detection from ECG Signals

Detecting obstructive sleep apnea from single-lead ECG recordings using signal processing, heart rate variability (HRV) features and machine learning.

This work was published in **BP International (2024)**: https://stm.bookpi.org/RHST-V9/article/view/11635

---

## Overview

Sleep apnea is usually diagnosed with an overnight polysomnography test, which is expensive and hard to access. This project explores whether a single ECG signal is enough to tell apnea patients apart from normal subjects, using the PhysioNet Apnea-ECG Database.

## Pipeline

| Step | What it does |
|---|---|
| 1. Data loading | Reads ECG records and apnea annotations from the PhysioNet Apnea-ECG Database using `wfdb` (100 Hz sampling) |
| 2. Filtering | Butterworth band-pass filter (8–16 Hz) to keep the QRS energy and remove noise |
| 3. Smoothing | 5-point moving-average filter |
| 4. Baseline removal | Two cascaded median filters (0.2 s and 0.6 s) to remove baseline wander |
| 5. R-peak detection | Adaptive-threshold QRS detection with signal/noise peak tracking and missed-beat search-back |
| 6. HRV features | Mean heart rate, median RR, SDNN, RMSSD, SDSD, NN50, NN70, NN90 |
| 7. Classification | 8 classifiers compared: SVM, Naive Bayes, Logistic Regression, KNN, Random Forest, Decision Tree, LDA and a Keras neural network |

## Repository structure

```
Apnea_ECG_ML/
├── notebooks/
│   ├── 01_ecg_preprocessing_qrs_detection.ipynb   # filtering, R-peak detection, HRV on one record
│   └── 02_hrv_features_and_classification.ipynb   # feature extraction for all records + model comparison
├── models/
│   └── naive_bayes_model.pkl                      # trained Gaussian Naive Bayes model
├── requirements.txt
└── README.md
```

## Results

34 records were used (a01–a20, b01–b04, c01–c10), with an 80/20 train/test split.

| Model | Test accuracy |
|---|---|
| SVM (linear) | 7/7 (100%) |
| Gaussian Naive Bayes | 7/7 (100%) |
| Logistic Regression | 6/7 (85.7%) |
| KNN (k=5) | 5/7 (71.4%) |
| LDA | 4/7 (57.1%) |
| Neural network (Keras) | 4/7 (57.1%) |
| Decision Tree | 3/7 (42.9%) |
| Random Forest | 2/7 (28.6%) |

**Limitations**

- The test set is very small (7 records), so these accuracy figures are not statistically reliable. A larger evaluation with cross-validation is needed.
- Features are taken from the first 60 seconds (6,000 samples) of each recording, and classification is per record, not per minute.
- The neural network overfits (100% training accuracy vs 57% validation accuracy).

## How to run

1. Clone this repository and install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Download the [Apnea-ECG Database](https://physionet.org/content/apnea-ecg/1.0.0/) from PhysioNet and place it at:
   ```
   notebooks/data/apnea-ecg-database-1.0.0/
   ```
3. Open the notebooks in order from the `notebooks` folder.

Originally developed with Python 3.8 and scikit-learn 0.24. The saved model may need retraining on newer scikit-learn versions.

## Dataset

T. Penzel, G.B. Moody, R.G. Mark, A.L. Goldberger, J.H. Peter. *The Apnea-ECG Database.* Computers in Cardiology 2000; 27:255–258. Available on [PhysioNet](https://physionet.org/content/apnea-ecg/1.0.0/).

## Tech stack

Python · NumPy · SciPy · pandas · wfdb · scikit-learn · Keras/TensorFlow · Matplotlib · Seaborn

## Author

**Shreya Pattan**: [LinkedIn](https://www.linkedin.com/in/shreya-pattan)
