# PQF (Projected Quantum Features) pilot on BPIC 2019

This repo contains a pilot experiment for **supply chain anomaly detection** using **Projected Quantum Features (PQF)** on the **BPI Challenge 2019** procurement event log.

Goal: compare a PQF-like **quantum fidelity kernel** + **One-Class SVM** against simple classical baselines under a **strict low false-positive budget**.

## What this repo implements

The pilot follows an operations-focused evaluation protocol:

- Chronological **train / validation / test** split with a **guard band**
- **Robust scaling** (median + MAD), fit on train only
- Threshold calibration on **validation normals** to target **α = 0.01** false-positive rate
- Metrics: **PR-AUC**, plus **Precision@α** and **Recall@α**
- Classical baselines:
  - Robust Z-score scoring
  - PCA reconstruction error
  - KMeans distance-to-centroid
- Quantum method:
  - PQF-like **fidelity kernel** computed with **Qiskit statevector simulation**
  - **One-Class SVM** trained on the precomputed kernel
- Labels:
  - BPIC 2019 has no anomaly annotations in this setup
  - The notebook uses **controlled anomaly injection** to create proxy labels for evaluation

## Repository contents

- `pqf_bpic2019_pilot_fixed2.ipynb`  
  Main notebook. It loads the log, builds case-level features, runs all methods, calibrates thresholds, and saves outputs.

## Data

The notebook expects a CSV file:

- `BPI_Challenge_2019.csv`


