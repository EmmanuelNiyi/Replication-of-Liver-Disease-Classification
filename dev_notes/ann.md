# Notes
### 22-11-25, 08:37

### FINAL RESULTS:

**ANN WITH SMOTE** 

| case       | accuracy | recall | precision | f1     | mcc    |
|------------|----------|--------|-----------|--------|--------|
| UNBALANCED | 0.7011   | 0.9473 | 0.7210    | 0.8180 | 0.0746 |
| BAL-AUG    | 0.7048   | 0.5588 | 0.7878    | 0.6455 | 0.4304 |
| DD-AUG     | 0.7789   | 0.6831 | 0.8479    | 0.7486 | 0.5736 |
| TD-AUG     | 0.8261   | 0.7704 | 0.8681    | 0.8143 | 0.6581 |
| QD-AUG     | 0.8660   | 0.8265 | 0.8992    | 0.8603 | 0.7357 |


**ANN WITH CTGAN**
| case    | accuracy | recall | precision | f1     | mcc     |
| ------- | -------- | ------ | --------- | ------ | ------- |
| BAL-AUG | 0.7482   | 0.9952 | 0.7497    | 0.8552 | 0.0567  |
| DD-AUG  | 0.7922   | 0.9654 | 0.8141    | 0.8822 | -0.0009 |
| TD-AUG  | 0.8209   | 0.9779 | 0.8358    | 0.9000 | 0.0342  |
| QD-AUG  | 0.8377   | 1.0000 | 0.8377    | 0.9117 | 0.0000  |


### SUMMARY:
Predictions using the ANN augmented with CTGAN produced results comparable to SMOTE augmentation. Interestingly, it had an MCC of 0 but a recall of 100% after generating four times the initial data. This shows that the CTGAN-augmented dataset was heavily imbalanced, causing the model to predict “true” for all cases—hence the perfect recall. Accuracy and precision still appeared high because there were overwhelming numbers of True Positives and only a few False Positives, but no True Negatives or False Negatives. The MCC was the giveaway: the absence of true or false negatives inevitably drives MCC to 0.
Examining the ANN + SMOTE results, Initially, the model behaves similarly to the GAN setup, showing extremely high recall (99%) and an MCC close to zero (0.05), indicating that the model is still essentially guessing “positive” most of the time. However, once the data is properly balanced with SMOTE, the MCC rises to 0.4 and recall drops almost by half, as the model begins to make negative predictions—leading to more False Negatives and False Positives. As the model continues training on more balanced data, recall improves again without a corresponding drop in MCC, suggesting the dataset remains consistently balanced.
In the end, the final metrics tell the full story: an accuracy of 0.87, recall of 0.83, precision of 0.90, and F1 score of 0.86, while the MCC rises significantly to 0.74.