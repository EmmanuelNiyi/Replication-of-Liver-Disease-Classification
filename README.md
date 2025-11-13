# Replication and Evaluation of Tabular Data Augmentation Techniques for Liver Disease Diagnosis Using ILPD Dataset
### **Project Overview**

This notebook aims to replicate key parts of the paper: “Tabular Data Generation to Improve Classification of Liver Disease Diagnosis” by _Mohammad Alauthman et al._

The original study evaluated the performance of several machine learning algorithms on the Indian Liver Patient Dataset (ILPD) while examining the impact of two data augmentation strategies: Generative Adversarial Networks (GANs) and the Synthetic Minority Oversampling Technique (SMOTE).

Their results indicate that K-Nearest Neighbors (KNN) combined with SMOTE achieved the highest overall classification accuracy. More broadly, the study reported that SMOTE consistently outperformed GAN across all augmentation scenarios (NO-AUG, DD-AUG, and TD-AUG).

However, the authors also note that GAN-based augmentation provides greater model stability compared to SMOTE, even if its absolute accuracy is lower.

### **Replication Objective**

In this notebook, I will:

1.  Progressively apply and compare several supervised machine learning algorithms — KNN, Logistic Regression, Decision Tree, SVM, and ANN — on the ILPD dataset.
2.  Evaluate model performance before and after applying SMOTE to assess the impact of oversampling on classification quality.
3.  Reproduce and analyze key evaluation metrics, including Accuracy, Precision, Recall, F1-Score, and ROC-AUC, to verify or challenge the findings reported in the original study.
