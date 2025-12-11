# Dev Notes
### 19-11-25, 20:42

### FINAL RESULTS:

**SVM WITH SMOTE**

| case       | accuracy | recall | precision | f1     | mcc     |
|------------|----------|--------|-----------|--------|---------|
| UNBALANCED | 0.7062   | 0.9879 | 0.7117    | 0.8273 | -0.0127 |
| BAL-AUG    | 0.7193   | 0.5298 | 0.8557    | 0.6441 | 0.4764  |
| DD-AUG     | 0.7464   | 0.5530 | 0.9064    | 0.6786 | 0.5377  |
| TD-AUG     | 0.7647   | 0.5936 | 0.9045    | 0.7153 | 0.5646  |
| QD-AUG     | 0.7642   | 0.5904 | 0.9068    | 0.7129 | 0.5647  |



**SVM WITH CTGAN**

|   | case    | accuracy | recall | precision | f1     | mcc     |
|---|---------|----------|--------|-----------|--------|---------|
| 0 | BAL-AUG | 0.7470   | 1.0000 | 0.7470    | 0.8552 | 0.0000  |
| 1 | DD-AUG  | 0.7789   | 0.9500 | 0.8103    | 0.8722 | -0.0183 |
| 2 | TD-AUG  | 0.8012   | 0.9486 | 0.8362    | 0.8852 | 0.0162  |
| 3 | QD-AUG  | 0.8136   | 0.9613 | 0.8407    | 0.8924 | 0.0165  |


### SUMMARY:
SVM shows the same pattern as Logistic Regression: SMOTE helps create balanced, reliable performance, while CTGAN inflates recall but destroys class balance.

SMOTE results:
- Accuracy gradually improves from 0.71 → 0.76 as oversampling increases.
- Recall drops from an overfitted 0.99 (unbalanced) to a more realistic ~0.59 once classes are balanced.
- Precision becomes very high (0.90+) with heavier SMOTE.
- MCC rises from negative (bad) to solid values around 0.56, indicating genuinely better-balanced decision boundaries.

CTGAN results:
- Accuracy increases to ~0.81, and recall remains extremely high (0.95–1.00).
- However, MCC is essentially 0, even dipping slightly negative at times — meaning the classifier isn’t learning a real boundary and is dominated by GAN-induced imbalance.
- High recall + near-zero MCC again confirms GAN-generated data is distorted/unbalanced for SVM.

Overall:
- SMOTE produces meaningful, stable improvements for SVM, with MCC showing strong gains.
- CTGAN results are misleadingly good on surface metrics but fundamentally unreliable due to class imbalance and poor separability in generated data.