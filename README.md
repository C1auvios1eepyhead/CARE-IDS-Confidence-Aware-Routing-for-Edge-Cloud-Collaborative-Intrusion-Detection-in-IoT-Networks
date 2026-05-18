# CARE-IDS: Confidence-Aware Routing for Edge-Cloud Collaborative Intrusion Detection

CARE-IDS is a confidence-aware selective offloading framework for edge-cloud collaborative intrusion detection in IoT networks. The framework first processes each traffic sample using a lightweight edge detector and then selectively invokes a stronger cloud detector when the edge prediction is estimated to be unreliable.

This repository provides the experimental implementation of CARE-IDS, including edge-only inference, cloud-only inference, confidence-aware routing, fixed-threshold routing analysis, and router feature ablation.

## Overview

Efficient IoT intrusion detection requires a balance between detection performance and deployment cost. Edge-side models provide fast local inference but may have limited classification capability, especially for difficult or minority attack categories. Cloud-side models usually provide stronger detection performance but require more computation and full cloud invocation.

CARE-IDS follows an edge-first and cloud-assisted strategy. Each sample is first processed by a lightweight Logistic Regression edge detector. A confidence-aware router then decides whether the edge prediction should be accepted locally or whether the sample should be offloaded to a Random Forest cloud detector.

In the evaluated DataSense eight-class setting, CARE-IDS achieves a favorable trade-off between detection performance and cloud usage by preserving most of the cloud detector's performance while reducing cloud invocations.

## Framework

The CARE-IDS pipeline consists of three main components:

- **Edge detector**: a lightweight Logistic Regression classifier that performs first-stage inference for all samples.
- **Cloud detector**: a Random Forest classifier used as a stronger fallback model for uncertain samples.
- **Confidence-aware router**: a Logistic Regression router trained from out-of-fold edge predictions to estimate whether the edge prediction is reliable.

During inference, CARE-IDS uses the following decision process:

1. The edge detector outputs a predicted class and class probability distribution.
2. Uncertainty-related features are extracted from the edge output.
3. The router decides whether to accept the edge prediction or invoke the cloud detector.
4. The final prediction is produced by either the edge detector or the cloud detector.

## Key Features

- Edge-cloud collaborative intrusion detection
- Confidence-aware sample-level offloading
- Out-of-fold router supervision to reduce optimistic bias
- Fixed-threshold routing analysis
- Router feature ablation with confidence, margin, entropy, and predicted-class information
- 10-fold stratified cross-validation evaluation
- Model-side inference time and cloud invocation analysis

## Dataset

The experiments are conducted on the DataSense dataset, a real-time sensor-based benchmark dataset for IIoT attack analysis. The original labels are mapped into an eight-class classification setting consisting of benign traffic and seven attack categories:

- Benign
- DDoS
- DoS
- Reconnaissance
- MITM
- Web-based attacks
- Brute-force attacks
- Malware

Non-numeric and identifier-related columns are removed before model training. Only numeric statistical features are retained. Missing and infinite values are replaced with zero, and feature columns are sorted to ensure consistent ordering across experimental runs.

Please obtain the dataset from the original source and place it under the local data directory before running the experiments.

## Repository Structure

```text
CARE-IDS/
├── data/
│   └── README.md
├── src/
│   ├── preprocessing.py
│   ├── models.py
│   ├── routing.py
│   ├── evaluation.py
│   └── utils.py
├── experiments/
│   ├── run_edge_only.py
│   ├── run_cloud_only.py
│   ├── run_care_ids.py
│   ├── run_threshold_analysis.py
│   └── run_router_ablation.py
├── results/
│   ├── figures/
│   └── tables/
├── requirements.txt
├── README.md
└── LICENSE
