# Apnea_ECG_ML
This project detects Obstructive Sleep Apnea (OSA) from ECG signals using signal processing, feature extraction, and machine learning. ECG data from PhysioNet is filtered and analyzed through RR-interval and HRV features. Models like SVM and KNN classify sleep intervals as apnea or non-apnea.
# Objective
Remove noise from raw ECG signals
Extract statistical and HRV-based features
Classify segments into Apnea vs Non-Apnea
# Methodology
| Step                      | Description                                           |
| ------------------------- | ----------------------------------------------------- |
| 1. Data Acquisition   | ECG signals collected from *PhysioNet* database       |
| 2. Preprocessing      | DC drift cancellation, low-pass & high-pass filtering |
| 3. Feature Extraction | RR intervals, HRV metrics, time & frequency features  |
| 4. Classification     | SVM, KNN trained for binary classification            |
