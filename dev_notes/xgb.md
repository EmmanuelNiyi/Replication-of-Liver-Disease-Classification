## Notes
### 22-11-25, 08:37

### FINAL RESULTS:

**XGB WITH SMOTE** 

| case       | accuracy | recall | precision | f1     | mcc    |
|------------|----------|--------|-----------|--------|--------|
| UNBALANCED | 0.8075   | 0.1140 | 0.2407    | 0.1138 | 0.0674 |
| BAL-AUG    | 0.8422   | 0.8872 | 0.8180    | 0.8496 | 0.6893 |
| DD-AUG     | 0.9224   | 0.9434 | 0.9062    | 0.9241 | 0.8462 |
| TD-AUG     | 0.9461   | 0.9624 | 0.9321    | 0.9469 | 0.8928 |
| QD-AUG     | 0.9569   | 0.9672 | 0.9477    | 0.9573 | 0.9140 |

**XGB WITH CTGAN**

| case    | accuracy | recall | precision | f1     | mcc    |
|---------|----------|--------|-----------|--------|--------|
| BAL-AUG | 0.7337   | 0.3143 | 0.4354    | 0.3407 | 0.2109 |
| DD-AUG  | 0.7807   | 0.1860 | 0.2953    | 0.2031 | 0.1215 |
| TD-AUG  | 0.8072   | 0.1481 | 0.1970    | 0.1481 | 0.0824 |
| QD-AUG  | 0.8075   | 0.1140 | 0.2407    | 0.1138 | 0.0674 |

### SUMMARY:

Summary:
XGBoost responds extremely well to SMOTE and extremely poorly to CTGAN, showing the clearest separation of all models you've tested.
SMOTE results:
- Massive jump in performance once the dataset is balanced.
- Accuracy climbs from 0.81 → 0.96, and recall from 0.11 → 0.97.
- MCC rises from 0.07 → 0.91, showing strong, stable decision boundaries.
- Higher oversampling levels consistently improve all metrics, with QD-SMOTE giving the best overall performance.

CTGAN results:
- All metrics collapse: recall falls to ~0.11–0.31, precision stays low (0.19–0.43), and MCC stays weak (0.06–0.21).
- Performance gets worse with more GAN data, suggesting the synthetic samples are fundamentally poor quality or distorted for tree-based models.

Overall:
- XGBoost is your strongest model so far, especially with heavy SMOTE.
- CTGAN data is unusable for XGB — it degrades the model drastically.
- Next I’ll switch to a proper train–validation split, apply SMOTE only to the training portion, and then fine-tune XGBoost’s hyperparameters before evaluating performance on the untouched validation set.