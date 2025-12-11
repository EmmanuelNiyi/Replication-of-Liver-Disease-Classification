# **Per-Model Results**

---

## **1 K-Nearest Neighbors (KNN)**

**SMOTE Summary:**

Performance increases sharply with SMOTE, especially at higher augmentation levels. Accuracy, precision, recall, and F1 improve steadily, and MCC grows from weak values in the unbalanced dataset to very strong values under heavy SMOTE. By the highest augmentation level, KNN achieves high accuracy, strong class separation, and stable behaviour across folds.

**GAN/CTGAN Summary:**

GAN-augmented training data yields high recall and strong F1 scores, but MCC remains consistently low. This indicates that the model is benefiting from inflated positive predictions caused by class imbalance in the synthetic data rather than learning better decision boundaries.

**Interpretation:**

KNN responds extremely well to SMOTE and poorly to CTGAN-style augmentation. SMOTE produces reliable improvements in performance and balance, while CTGAN metrics are misleading due to imbalance.

---

## **2 Decision Tree (DT)**

**SMOTE Summary:**

Decision Trees benefit significantly from SMOTE. Increasing the oversampling level steadily raises accuracy and F1, and MCC shows major improvement, eventually reaching strong values. StratifiedKFold also stabilizes recall across folds and corrects earlier failures caused by minority-class absence.

**GAN/CTGAN Summary:**

GAN-augmented datasets generate moderate improvements in accuracy and recall but consistently low MCC. This indicates that the tree is overfitting to an imbalanced synthetic distribution and failing to learn a balanced classification boundary.

**Interpretation:**

Decision Trees show robust gains under SMOTE, with much stronger class discrimination and reliable MCC progression. CTGAN produces only superficial improvements and unreliable model behaviour.

---

## **3 Logistic Regression (LR)**

**SMOTE Summary:**

SMOTE produces modest but stable changes. Accuracy remains close to the baseline, but MCC improves substantially, indicating better balance between classes. Recall drops after balancing but gradually improves with higher oversampling levels.

**GAN/CTGAN Summary:**

GAN-based augmentation inflates recall to extremely high values, often above 0.97, while MCC collapses toward zero. This reflects a near-complete collapse toward predicting the majority class.

**Interpretation:**

Logistic Regression remains relatively rigid and benefits mainly through improved class balance rather than improved accuracy. SMOTE produces usable, balanced behaviour; GAN augmentation produces the strongest signs of mode collapse.

---

## **4 Support Vector Machine (SVM)**

**SMOTE Summary:**

SMOTE steadily improves class balance for SVM. Accuracy increases gradually, precision becomes very strong, and MCC rises from negative or near-zero values to moderate–strong values as oversampling increases. Recall stabilizes at realistic levels once the dataset becomes balanced.

**GAN/CTGAN Summary:**

GAN-augmented data yields extremely high recall but near-zero MCC, indicating unreliable class separation. Accuracy appears to increase, but this is misleading because the model is largely predicting the majority class.

**Interpretation:**

SVM gains meaningful improvements from SMOTE and becomes significantly more balanced. GAN-based augmentation consistently produces distorted class boundaries and unstable performance.

---

## **5 Artificial Neural Network (ANN)**

**SMOTE Summary:**

ANNs benefit from increased and balanced data. With heavier SMOTE, accuracy improves, recall becomes more stable, precision strengthens, and MCC increases significantly. The model transitions from majority-class bias to more balanced decision-making.

**GAN/CTGAN Summary:**

GAN augmentation causes ANN metrics to inflate sharply, particularly recall, which reaches perfect or near-perfect values. However, MCC remains close to zero, demonstrating that the model is not learning meaningful negative-class distinctions.

**Interpretation:**

ANNs respond well to SMOTE and poorly to GAN-based data. SMOTE produces gradual, reliable improvements in performance and class balance, while GAN augmentation leads to prominent mode collapse.

---

## **6 XGBoost (XGB)**

**SMOTE Summary:**

XGBoost shows the largest improvement under SMOTE of any model tested. Performance rises sharply with each augmentation level, culminating in very high accuracy, precision, recall, and MCC. Class boundaries become more stable and expressive with balanced training data.

**GAN/CTGAN Summary:**

GAN-generated data severely degrades XGBoost. Recall drops to very low values, precision becomes unstable, and MCC collapses. Performance becomes worse with more GAN samples, indicating that CTGAN produces data distributions that are poorly aligned with tree-based learners.

**Interpretation:**

XGBoost is the strongest overall model when trained with SMOTE and the most sensitive to CTGAN’s imbalance issues. SMOTE and XGB complement each other exceptionally well, while GAN-based augmentation consistently harms performance.

---