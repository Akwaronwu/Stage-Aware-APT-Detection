# Stage-Aware APT Detection and Digital Trust Assessment

A stage-aware behavioural intrusion detection framework for identifying anomalous network flows using an unsupervised Isolation Forest, interpreting detected behaviours through the Advanced Persistent Threat (APT) lifecycle, and deriving an evidence-based Digital Trust assessment.

## Overview

Advanced Persistent Threats (APTs) are stealthy, prolonged, and multi-stage cyberattacks that may evade conventional signature-based intrusion detection because malicious behaviour can resemble legitimate network activity or deviate from previously observed attack signatures.

This research develops a **stage-aware behavioural detection framework** that extends flow-level anomaly detection beyond a binary *normal/anomalous* decision.

The framework separates the analysis into three major components:

1. **Behavioural Anomaly Detection** — Isolation Forest is trained exclusively on benign network traffic to learn a baseline of normal behaviour.
2. **APT Lifecycle Interpretation** — detected anomalous flows are associated with relevant APT lifecycle stages using their attack categories and behavioural characteristics.
3. **Digital Trust Assessment** — measurable evidence from anomaly scores, behavioural deviation, consistency, and stage context is used to provide a contextual trust-oriented security assessment.

The approach is designed so that **detection, stage interpretation, and trust assessment remain analytically distinct**. APT-stage mapping and Digital Trust assessment do not alter the underlying Isolation Forest detector.

---

## Research Objectives

The study aims to:

* Develop a benign-trained behavioural anomaly detection model using Isolation Forest.
* Engineer network-flow features capable of representing behavioural deviations.
* Detect malicious flows without using malicious labels during model training.
* Map detected anomalous behaviours to relevant APT lifecycle stages.
* Develop an evidence-based Digital Trust assessment layer.
* Evaluate detection performance across attack categories and lifecycle stages.
* Improve the contextual interpretability of anomaly detection outputs.

---

## Dataset

The study uses the **NF-UQ-NIDS-v2 NetFlow dataset**, which contains normal and malicious network-flow records representing multiple attack categories.

The dataset includes attack categories such as:

* Scanning / Reconnaissance
* Brute Force
* Exploits
* Malicious Code Execution
* Backdoors
* Internal Compromise
* Worms
* Man-in-the-Middle (MITM)
* Data Extraction
* Ransomware

The dataset is used as a network intrusion detection dataset rather than as a collection of complete APT campaigns.

### Important Dataset Limitation

NF-UQ-NIDS-v2 does not contain Digital Trust labels and its malicious traffic does not necessarily represent complete, real-world APT campaigns.

Therefore:

> **APT lifecycle-stage mapping in this study is an analytical interpretation of attack categories and behavioural characteristics, not ground-truth labelling of complete APT campaigns.**

Similarly, Digital Trust is treated as a **derived contextual and decision-support dimension**, rather than an observed ground-truth label.

---

## APT Lifecycle Mapping

Detected anomalous flows are interpreted according to their attack category and behavioural profile.

| Attack Behaviour / Category | Interpreted APT Stage |
| --------------------------- | --------------------- |
| Scanning / Reconnaissance   | Reconnaissance        |
| Brute Force                 | Initial Access        |
| Exploits                    | Initial Access        |
| Malicious Code Execution    | Execution             |
| Backdoors                   | Persistence           |
| Internal Compromise         | Internal Compromise   |
| Worms                       | Lateral Movement      |
| MITM                        | Lateral Movement      |
| Data Extraction             | Data Exfiltration     |
| Ransomware                  | Impact / Disruption   |

The mapping is performed **after anomaly detection** and is not used as a feature for training the Isolation Forest.

---

## Methodology

The proposed workflow consists of the following stages:

```text
NF-UQ-NIDS-v2
       │
       ▼
Data Cleaning
       │
       ├── Missing-value analysis
       ├── Duplicate detection
       └── Data consistency checks
       │
       ▼
Behaviour-Oriented Feature Engineering
       │
       ├── Flow statistics
       ├── Protocol attributes
       ├── Communication patterns
       └── Session characteristics
       │
       ▼
Feature Scaling
       │
       ▼
Benign Traffic
       │
       ▼
Isolation Forest
       │
       ▼
Anomaly Score
       │
       ▼
Threshold Selection
       │
       ▼
Anomalous Flows
       │
       ├───────────────┐
       ▼               ▼
APT Stage Mapping   Digital Trust Assessment
       │               │
       └───────┬───────┘
               ▼
      Contextual Security Intelligence
```

---

## Isolation Forest Detection

Isolation Forest is used as the primary anomaly detection algorithm.

The model is trained **exclusively on benign traffic**, establishing a representation of normal network behaviour.

During inference, each flow receives an anomaly-related score based on how easily it can be isolated from the learned normal population.

Flows exceeding the selected detection threshold are classified as anomalous and passed to the interpretation stage.

The Isolation Forest is responsible only for **anomaly detection**. It does not directly determine the APT lifecycle stage.

---

## Digital Trust Assessment

Digital Trust is operationalized as a **contextual decision-support dimension** derived from measurable evidence available within the dataset and detection framework.

Potential evidence includes:

* Degree of deviation from learned normal behaviour
* Isolation Forest anomaly score
* Behavioural consistency
* Associated attack category
* Interpreted APT lifecycle stage
* Supporting flow-level characteristics

The framework can therefore characterize network behaviour using contextual outcomes such as:

* **Trustworthy**
* **Suspicious**
* **Requires Further Investigation**

These categories are intended to support security decision-making rather than represent ground-truth trust labels.

Where a trust attribute cannot be directly measured from network-flow data, it is explicitly treated as a limitation rather than inferred as ground truth.

---

## Evaluation

Detection performance will be evaluated using:

* Precision
* Recall
* F1-score
* ROC-AUC
* Accuracy

Accuracy is reported alongside the other metrics because the dataset contains class imbalance.

Performance will also be examined by:

* Attack category
* APT lifecycle stage
* Anomaly-score distribution
* Behavioural characteristics
* Detection difficulty

This analysis is intended to identify behaviours that are easier or harder for the Isolation Forest to isolate and to examine cases where malicious traffic resembles normal network behaviour.

---

## Implementation

The framework is implemented in:

* **Python**
* **Jupyter Notebook**
* **Scikit-learn**
* **Pandas**
* **NumPy**
* **Matplotlib**
* **Seaborn** (where applicable)
* Additional Python libraries used for statistical analysis and visualization

---

## Repository Structure

```text
Stage-Aware-APT-Detection/
│
├── Complete_dataset_v2.ipynb
├── README.md
└── .gitignore
```

The main research implementation is currently contained in:

```text
Complete_dataset_v2.ipynb
```

The notebook contains the data-processing, feature-engineering, anomaly-detection, evaluation, and analytical workflow for the study.

---

## Reproducibility

The project is intended to support reproducible research.

The analysis pipeline separates:

1. Data preparation
2. Feature engineering
3. Normal-behaviour modelling
4. Anomaly detection
5. Threshold selection
6. Attack-category analysis
7. APT lifecycle interpretation
8. Digital Trust assessment
9. Performance evaluation
10. Visualization and interpretation

The full NF-UQ-NIDS-v2 dataset is **not included in this repository** because of dataset size and distribution considerations.

Researchers wishing to reproduce the analysis should obtain the appropriate dataset independently and configure the notebook accordingly.

---

## Research Contributions

The study is expected to contribute:

### 1. Reproducible Detection Pipeline

A pipeline that separates anomaly detection from subsequent APT lifecycle interpretation and Digital Trust assessment.

### 2. Behavioural Analysis of Attack Categories

Empirical evidence showing how effectively flow-level anomaly scores distinguish malicious behaviours from normal traffic across different attack categories.

### 3. Contextual Digital Trust Model

An operational approach to Digital Trust based on measurable behavioural evidence rather than an assumed ground-truth trust label.

### 4. Explainable Security Outputs

A framework for transforming a basic anomaly detection result into contextual information concerning likely attack stage, supporting evidence, and security significance.

---

## Limitations

Several limitations are explicitly recognized:

* NF-UQ-NIDS-v2 is an intrusion detection dataset and does not represent complete APT campaigns.
* The dataset contains no Digital Trust ground-truth labels.
* APT lifecycle stages are analytically mapped from attack categories and behavioural characteristics.
* Flow-level data may not capture the complete temporal progression of a real APT campaign.
* Digital Trust cannot be fully measured from network-flow information alone.
* The proposed trust assessment should therefore be interpreted as decision support rather than a definitive measure of trustworthiness.

---

## Future Work

Future research may extend the framework through:

* Validation using real-world APT campaign datasets
* Temporal and sequential modelling of attack behaviour
* Graph-based network behaviour analysis
* Comparison with other unsupervised anomaly detection algorithms
* Semi-supervised and ensemble detection approaches
* Explainable AI techniques
* Entity-level rather than flow-level trust modelling
* Real-time deployment for security monitoring
* Integration with Security Information and Event Management (SIEM) systems

---

## Research Status

**Status:** Active Research / Development

This repository contains the computational implementation and experimental workflow associated with the research project.

The methodology and results are subject to ongoing refinement and validation.

---

## Author

**Bright Gazie Akwaronwu, MCPN**
Faculty Member, Department of Information Technology
Babcock University, Nigeria

**Research interests:** Machine Learning for Cybersecurity, Network Security and Intrusion Detection, Cryptography, and Biomedical Informatics.

---

## Citation

If this repository contributes to your research, please cite the associated research work when the formal publication becomes available.

---

## License

A license will be added when the appropriate terms for the research code and accompanying materials have been determined.
