# Dev Notes
### 19-11-25, 13:18

### FINAL RESULTS:

**LR WITH SMOTE**

|   | case       | accuracy | recall | precision | f1     | mcc    |
|---|------------|----------|--------|-----------|--------|--------|
| 0 | UNBALANCED | 0.7184   | 0.9278 | 0.7436    | 0.8243 | 0.1731 |
| 1 | BAL-AUG    | 0.7253   | 0.5709 | 0.8274    | 0.6641 | 0.4776 |
| 2 | DD-AUG     | 0.7319   | 0.5928 | 0.8226    | 0.6826 | 0.4860 |
| 3 | TD-AUG     | 0.7325   | 0.5968 | 0.8206    | 0.6894 | 0.4844 |
| 4 | QD-AUG     | 0.7304   | 0.5946 | 0.8167    | 0.6867 | 0.4795 |



**LR WITH CTGAN**

|   | case    | accuracy | recall | precision | f1     | mcc    |
|---|---------|----------|--------|-----------|--------|--------|
| 0 | BAL-AUG | 0.7482   | 0.9871 | 0.7531    | 0.8541 | 0.0745 |
| 1 | DD-AUG  | 0.8042   | 0.9838 | 0.8144    | 0.8906 | 0.0262 |
| 2 | TD-AUG  | 0.8229   | 0.9837 | 0.8338    | 0.9018 | 0.0198 |
| 3 | QD-AUG  | 0.8247   | 0.9799 | 0.8384    | 0.9026 | 0.0040 |



### SUMMARY:
Summary:
Logistic Regression reacts very differently from KNN to augmentation.

SMOTE results:
- Accuracy stays around 0.72–0.73 regardless of oversampling level.
- Precision improves with balancing, but recall drops sharply (from ~0.93 to ~0.59).
- MCC improves from 0.17 → ~0.48, showing better class balance handling even though overall predictive power doesn’t meaningfully rise.

CTGAN results:
- Accuracy climbs as high as 0.82, and recall becomes extremely high (~0.98), but
- MCC collapses to nearly zero, meaning the classifier is essentially predicting one class most of the time.
- Strong recall + very low MCC indicates severe class imbalance or mode collapse in the GAN-generated data.

Overall:
- SMOTE yields stable, balanced behavior for Logistic Regression, though only modest gains.
- CTGAN boosts accuracy/recall artificially but produces unreliable, low-quality datasets for LR, making its outputs unusable.