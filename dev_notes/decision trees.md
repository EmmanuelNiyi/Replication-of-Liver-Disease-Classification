### 19-11-25, 11:80
When applying my model, I tried running the notebook but replaced KNN with a Decision Tree. However, I ran into an error:

"Recall is ill-defined and being set to 0.0 due to no true samples. Use zero_division parameter to control this behavior."

This happened because some of the cross-validation splits didn’t contain any true values for one of the classes. As a result, recall couldn’t be calculated for that fold, which triggered the warning.

A quick workaround was to reduce the number of validation splits. In my case, the only value that didn’t throw the error was 2—but this defeats the purpose of cross-validation (although this approach is sometimes used for extremely imbalanced datasets).

A far better solution is to use StratifiedKFold, which ensures that each fold preserves the same class proportions as the full dataset. This prevents folds from having zero instances of the minority class, meaning recall and other class-wise metrics can be computed properly in every fold.

Using StratifiedKFold not only fixed the error but also improved the overall metrics, as shown below.

**Before SKF**

|   | case       | accuracy | recall | precision | f1     | mcc    |
|---|------------|----------|--------|-----------|--------|--------|
| 0 | UNBALANCED | 0.6547   | 0.7663 | 0.7490    | 0.7569 | 0.1400 |
| 1 | BAL-AUG    | 0.7373   | 0.4787 | 0.5808    | 0.5231 | 0.2301 |
| 2 | DD-AUG     | 0.8512   | 0.5780 | 0.6348    | 0.6045 | 0.2961 |
| 3 | TD-AUG     | 0.8876   | 0.6150 | 0.6529    | 0.6323 | 0.3619 |
| 4 | QD-AUG     | 0.9160   | 0.6428 | 0.6654    | 0.6530 | 0.3304 |

**After SKF**

|   | case       | accuracy | recall | precision | f1     | mcc    |
|---|------------|----------|--------|-----------|--------|--------|
| 0 | UNBALANCED | 0.6255   | 0.7469 | 0.7319    | 0.7379 | 0.0751 |
| 1 | BAL-AUG    | 0.7470   | 0.6941 | 0.7929    | 0.7325 | 0.5049 |
| 2 | DD-AUG     | 0.8566   | 0.8301 | 0.8816    | 0.8532 | 0.7172 |
| 3 | TD-AUG     | 0.8948   | 0.8876 | 0.9046    | 0.8949 | 0.7915 |
| 4 | QD-AUG     | 0.9148   | 0.9145 | 0.9199    | 0.9155 | 0.8327 |



### FINAL RESULTS:

**DT WITH SMOTE**

|   | case       | accuracy | recall | precision | f1     | mcc    |
|---|------------|----------|--------|-----------|--------|--------|
| 0 | UNBALANCED | 0.6255   | 0.7469 | 0.7319    | 0.7379 | 0.0751 |
| 1 | BAL-AUG    | 0.7470   | 0.6941 | 0.7929    | 0.7325 | 0.5049 |
| 2 | DD-AUG     | 0.8566   | 0.8301 | 0.8816    | 0.8532 | 0.7172 |
| 3 | TD-AUG     | 0.8948   | 0.8876 | 0.9046    | 0.8949 | 0.7915 |
| 4 | QD-AUG     | 0.9148   | 0.9145 | 0.9199    | 0.9155 | 0.8327 |



**DT WITH CTGAN**

|   | case    | accuracy | recall | precision | f1     | mcc    |
|---|---------|----------|--------|-----------|--------|--------|
| 0 | BAL-AUG | 0.6747   | 0.7784 | 0.7873    | 0.7790 | 0.1422 |
| 1 | DD-AUG  | 0.7169   | 0.8160 | 0.8356    | 0.8234 | 0.0786 |
| 2 | TD-AUG  | 0.7217   | 0.8100 | 0.8527    | 0.8273 | 0.0602 |
| 3 | QD-AUG  | 0.7446   | 0.8366 | 0.8591    | 0.8454 | 0.0761 |




### SUMMARY:
In summary, using a Decision Tree (DT) augmented with SMOTE consistently yielded high metrics (all above 91%, except MCC at 83%). However, these results still fell short of what was achieved with K-Nearest Neighbors (KNN), consistent with the reference paper.

Similarly, while CTGAN produced higher accuracy, recall, precision, and F1 scores, it still underperformed compared to SMOTE.