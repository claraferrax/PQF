# PQF (Projected Quantum Features) pilot on BPIC 2019

This repo contains a pilot experiment that tests **Projected Quantum Features (PQF)** for **operations-style anomaly detection** on the **BPI Challenge 2019** procurement event log.

The focus is the **low false-alarm regime**. In monitoring, false alarms quickly destroy trust, so the evaluation is designed around a strict alert budget.

## What this repo does

- Builds **case-level features** from the BPIC 2019 event log.
- Uses a **chronological train / validation / test** split with a **guard band** to reduce temporal leakage.
- Applies **robust preprocessing** (median + MAD), fit on train only.
- Calibrates thresholds on **validation normals** to target **α = 0.01** false-positive rate, then freezes the threshold for test.
- Reports **PR-AUC** plus **Precision@α** and **Recall@α** at the operating point.

## Methods included

### Classical baselines (transparent, low-overhead)
- Robust Z-score scoring (max absolute robust z across features)
- PCA reconstruction error
- K-means distance to nearest centroid

### PQF hybrid method
- PQF-style **quantum feature map** + **fidelity kernel** computed with **Qiskit statevector simulation**
- **One-Class SVM** trained on the precomputed kernel
- Feature dimension is aligned with a **fixed qubit budget** (pilot uses 8 qubits)

## Labels and evaluation setup

BPIC 2019 does not provide anomaly annotations for this setup, so the notebook uses a **controlled anomaly injection** strategy to create proxy labels for evaluation. Training stays unsupervised.

To reduce sensitivity to randomness, the pipeline is **repeated across multiple random seeds** and results are summarized with variability estimates.

## Repository contents

- `pqf_bpic2019_pilot_ci.ipynb`  
  Main notebook. Loads the log, builds features, runs baselines + PQF, calibrates thresholds at α=0.01, and reports metrics.
