# Multi-Task Cheminformatics Pipeline: Molecular Toxicity & FDA Approval Predictor

A high-performance Computational Chemistry and Machine Learning pipeline that performs **Multi-Task/Multi-Label Classification** on the benchmark **MoleculeNet ClinTox Dataset**. This model simultaneously predicts whether a given chemical compound is FDA-approved and evaluates its clinical trial toxicity using structural molecular fingerprints.

---

## 🧬 Project Overview & Core Concept
In computer-aided drug discovery (CADD), traditional virtual screening usually models a single molecular property at a time (Single-Task Learning). However, clinical candidate failures form a major bottleneck and are heavily driven by safety parameters. 

This project implements a **Multi-Output Machine Learning Framework** that takes raw chemical structures (**SMILES strings**) as input, and concurrently maps them to two distinct biological endpoints:
1. **FDA_APPROVED**: Regulatory approval status flag (Returns **1** if approved, **0** if not).
2. **CT_TOX**: Clinical trial toxicity failure flag (Returns **1** if toxic/failed, **0** if safe/passed).

---

## 🛠️ Tech Stack & Architecture
* **Language:** Python
* **Cheminformatics Core:** RDKit (Molecular serialization, validation, & feature extraction)
* **Machine Learning Framework:** Scikit-Learn (`MultiOutputClassifier`, `RandomForestClassifier`)
* **Data Processing & Visualization:** NumPy, Pandas, Matplotlib, Seaborn
* **Model Serialization:** Joblib

---

## 🧠 Data Engineering & Pipeline Workflow

### 1. Structure Tokenization & Validation
Raw SMILES strings are processed using RDKit to construct molecular objects. The pipeline features strict exception handling (`try-except` blocks) to catch and gracefully bypass faulty chemical structures (e.g., explicit valence violations or unkekulized aromatic graphs), outputting a fallback uniform 2048-bit bit array of zeros to maintain rigid matrix alignment and avoid shape mismatch crashes.

### 2. Feature Extraction (Molecular Signatures)
Molecules are featurized using **2048-bit Morgan Fingerprints** (Circular Fingerprints) with a radius of 2, establishing a dense chemical feature matrix $X$ of shape `(1484, 2048)`.

### 3. Multi-Task Target Mapping
Targets are stacked into a joint multi-label binary matrix $y$ of shape `(1484, 2)`.

---

## 📊 Evaluation Metrics & Performance Results
The dataset displays a realistic **Class Imbalance** (where safe and approved drugs form the majority). Despite this structural asymmetry, the model delivers strong discriminative thresholds evaluated across 80/20 train-test splits:

### 🟦 Endpoint 1: FDA Approval Prediction
* **Test Accuracy:** 92.59%
* **ROC-AUC Score:** 0.8391 *(Excellent discriminative capability)*

### 🟥 Endpoint 2: Clinical Trial Toxicity (CT_TOX)
* **Test Accuracy:** 91.92%
* **ROC-AUC Score:** 0.7737 *(Robust classification threshold)*

---



