# **Replicating “Tabular Data Generation to Improve Classification of Liver Disease Diagnosis” Using SMOTE and GAN-Based Augmentation**

---

## **1. Project Overview**

This project replicates and extends key components of the study *“Tabular Data Generation to Improve Classification of Liver Disease Diagnosis”* (Alauthman et al., 2023). The goal is to evaluate how data augmentation affects predictive performance for liver disease classification using the Indian Liver Patient Dataset (ILPD).

The original paper compared two augmentation techniques—Generative Adversarial Networks (GANs) and the Synthetic Minority Oversampling Technique (SMOTE)—across five machine learning algorithms. This replication follows that design closely while introducing two improvements:

- inclusion of the **Matthews Correlation Coefficient (MCC)** to assess model validity under class imbalance,
- extension of augmentation levels beyond the original 3× setup to **4×** for additional analysis.

CTGAN, a GAN architecture for tabular data, is used as the practical implementation of “GAN-based augmentation,” since the original paper did not specify its GAN framework in detail.

The findings reproduce most trends from the original paper: SMOTE consistently outperforms GAN-based augmentation, and KNN remains one of the strongest classifiers. However, the inclusion of MCC reveals hidden issues in GAN-generated datasets that were not discussed in the original study.

---

## **2. Dataset**

- **Indian Liver Patient Dataset (ILPD)**
- 583 total samples
- Strong class imbalance (majority: minority ≈ 4:1)
- 10 clinical/laboratory features
- Binary target indicating the presence or absence of liver disease

The dataset was used without modification.

---

## 3. Original Paper Summary

The study “Tabular Data Generation to Improve Classification of Liver Disease Diagnosis” (Alauthman et al., 2023) evaluates five machine learning algorithms—Logistic Regression, K-Nearest Neighbors, Decision Tree, Support Vector Machine, and Artificial Neural Networks—on the Indian Liver Patient Dataset (ILPD). The authors examined whether augmenting the dataset with two techniques, GAN-based synthetic data generation and the Synthetic Minority Oversampling Technique (SMOTE), could improve predictive performance for liver disease classification.

The results show consistent improvement when the training data is expanded. SMOTE outperforms GAN augmentation across all augmentation scenarios (NO-AUG, DD-AUG, and TD-AUG), with KNN achieving the strongest accuracy overall (reported averages near 99%). The authors also report that GAN-generated data exhibits higher stability across repeated runs, despite lagging behind SMOTE in raw accuracy.

While the paper provides accuracy, precision, recall, and F1, it does not include the Matthews Correlation Coefficient (MCC), which is critical when working with imbalanced datasets. GAN parameter settings are described at a high level, but the exact implementation details—such as the specific GAN framework used—are not reported in full.

---

## **4. Replication Methodology**

This replication follows the overall structure of the original study but adjusts several elements to address gaps in the published methodology.

The Indian Liver Patient Dataset (ILPD) was used without modification. Six algorithms were implemented: K-Nearest Neighbors, Decision Tree, Logistic Regression, Support Vector Machine, Artificial Neural Networks, and XGBoost (included for expanded analysis). Each algorithm was trained and evaluated across two augmentation families:

- **SMOTE oversampling** at four levels:
    - BAL-AUG (balanced 1×)
    - DD-AUG (double size)
    - TD-AUG (triple size)
    - QD-AUG (quadruple size)
    
    The original paper used augmentation up to 3× with results averaged across folds; this replication extended augmentation to 4× without averaging.
    
- **GAN-based augmentation**
    
    The paper did not specify which GAN architecture was used. To replicate the “GAN augmentation” component as faithfully as possible, the CTGAN implementation from the Synthetic Data Vault (SDV) library was chosen. CTGAN is optimized for tabular data and is the closest available analogue to the GAN pipeline described in the original study.
    

StratifiedKFold was used throughout to preserve class proportions within each split. 

### **Evaluation**

- Metrics: Accuracy, Precision, Recall, F1, MCC
- StratifiedKFold for consistent class distribution
- SHAP interpretability analysis for KNN

MCC was included to detect unreliable performance under class imbalance—a limitation of the original paper.

---

## **5. Results Summary Across Models**

### **High-Level Findings**

- **SMOTE produces reliable, stable improvements across all models.**
- **GAN-based augmentation inflates recall and accuracy but collapses MCC**, indicating severe class imbalance in synthetic data.
- **KNN and XGBoost respond most strongly to SMOTE**, reaching MCC values above 0.85 at higher augmentation levels.
- **Decision Trees and ANN show strong improvements under SMOTE**, though still weaker than KNN or XGB.
- **Logistic Regression and SVM gain modest improvements**, with meaningful MCC increases but limited accuracy gains.
- **GAN-generated data is unreliable for all models**, with MCC frequently near zero or even negative.

---

## **6. MCC and the Failure of GAN-Based Augmentation**

GAN-based augmentation (via CTGAN) produced datasets with severe class imbalance:

- Example: 2781 samples of class 1 vs. 539 samples of class 2.
- Standard accuracy and recall appeared strong but were misleading.
- MCC exposed the issue convincingly, often falling to **0.00–0.17**.

The original paper did not use MCC, which likely masked similar problems in their GAN-based experiments.

SMOTE, in contrast, explicitly controls class balance and produced consistent MCC improvements across all models.

---

## **7. Comparison with Original Paper Results**

- KNN + SMOTE remains the strongest method in both studies.
- The original paper reports ~99% accuracy; this replication achieved ~95%.
- Differences may stem from:
    - unspecified preprocessing in the original study
    - lack of explicit GAN architecture details
    - different randomness seeds
    - absence of MCC in the original analysis
- CTGAN proved unstable and unreliable compared to SMOTE.

Overall trends were successfully reproduced.

---

## **8. How to Run the Code**

1. Install dependencies
2. Run the notebooks in the sequence provided
3. Each notebook corresponds to one model and contains:
    - baseline results
    - SMOTE experiments
    - CTGAN experiments
    - aggregated comparison tables

A separate `dev_notes.md` file contains experiment logs.

---

## **9. Repository Structure**

```
project/
│── notebooks/
│── results/
│── dev_notes/
│── README.md
```

---

## **10. Limitations**

- CTGAN reproducibility limitations
- MCC sensitivity to extreme imbalance
- Small dataset increases oversampling risk
- High-level ambiguity in original GAN methodology
- Model parameter details partially unspecified in the original study

---

## **11. Conclusion**

This replication reinforces the original study’s main conclusion:

**SMOTE is a superior augmentation strategy for the ILPD dataset.**

Additional insights from MCC reveal that:

- GAN-based augmentation produces unreliable datasets with misleading surface metrics.
- True classifier performance can only be assessed with metrics that incorporate all confusion matrix components.
- KNN and XGBoost are the strongest models for this task, especially with heavier SMOTE augmentation.

The inclusion of MCC and the use of CTGAN highlight methodological challenges not addressed in the original paper and offer a clearer understanding of augmentation reliability in small, imbalanced medical datasets.

---

## **12. References**

**Original Study**: [Alauthman, M., et al. *Tabular Data Generation to Improve Classification of Liver Disease Diagnosis.* Applied Sciences, 13(4), 2678 (2023). https://doi.org/10.3390/app13042678](https://www.mdpi.com/2076-3417/13/4/2678)

**Dataset**: [UCI Machine Learning Repository. *Indian Liver Patient Dataset (ILPD).*](https://archive.ics.uci.edu/ml/datasets/ILPD+%28Indian+Liver+Patient+Dataset%29)

**CTGAN**: [Synthetic Data Vault (SDV). *CTGAN: Conditional Tabular GAN for Synthesizing Tabular Data.*](https://github.com/sdv-dev/CTGAN)
