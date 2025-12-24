# Exploratory Seismic Data Analysis & Prediction: Istanbul Case Study

This project presents an exploratory analysis designed to study seismic activity in the Istanbul region. Unlike traditional time-series forecasting, this study approaches earthquake prediction through signal processing and frequency-based feature engineering, transforming the problem into a risk classification task.

## Project Overview

The primary objective was to classify seismic windows into risk levels based on future magnitude potential. The study demonstrates that seismic data in this context behaves more like a signal processing problem than a cumulative time-series, with tree-based models significantly adapating better to frequency domain features than temporal deep learning architectures.

## Key Methodologies

### 1. Frequency-Based Feature Engineering
A critical finding of this study was that raw seismic data requires transformation into the frequency domain to yield predictive insights.
* **FFT (Fast Fourier Transform):** Used to extract dominant frequency content from magnitude sequences.
* **Wavelet Transformation (Db1, Level 2):** Captured multi-resolution energy representations using both frequency and time domains.
* **SVD (Singular Value Decomposition):** Extracted latent semantic features (dimensionality signature) from signal windows.
* **Geophysical Metrics:** Implementation of the Gutenberg-Richter Law (b-value/a-value estimation) and Seismic Energy calculations.

### 2. Unsupervised Anomaly Detection
Three algorithms were employed to detect unusual seismic patterns without labeled data:
* Isolation Forest
* One-Class SVM
* Local Outlier Factor (LOF)
* **Result:** Identified "Unanimous Anomalies"—events flagged by all three models, often correlating with high-energy release periods.

### 3. Model Benchmarking
We benchmarked classical machine learning vs. deep learning architectures:
* **Classical:** Logistic Regression, Random Forest, XGBoost, LightGBM.
* **Deep Learning:** MLP (Multi-Layer Perceptron) and LSTM (Long Short-Term Memory).

## Results Summary

The problem was framed as classifying the maximum magnitude of the next 30-event window into risk bins.

| Model | F1 Score (Macro) | Outcome |
| :--- | :--- | :--- |
| **Random Forest** | **0.931** | Best performance; captured non-linear feature interactions. |
| **XGBoost** | 0.921 | Highly competitive and robust. |
| **LightGBM** | 0.905 | Efficient training with high accuracy. |
| **MLP** | ~0.60 | Baseline performance. |
| **LSTM** | ~0.20 | **Failed to generalize.** |

### Experimental Insight: The LSTM Failure
Despite high training accuracy, LSTM models failed to generalize on test data (Acc < 22%). Experiments involving time-step shuffling revealed that the temporal order within short seismic windows carries significantly less information than the statistical and frequency-based features derived from the signal. This suggests the data does not possess strong Markov properties in this windowed format, explaining why tree-based models (using FFT/Wavelet features) outperformed sequential models.
