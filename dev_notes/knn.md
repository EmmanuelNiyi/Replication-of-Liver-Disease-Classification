## 1. Dev Notes
### 15-11-25, 3:00
I have been able to apply KNN to the ILP dataset and with an accuracy of 0.67 and precision of 0.80, however this is without cross validation or data augmentation 

Other Metrics:
- Accuracy: 0.667
- Precision: 0.802
- Recall: 0.739
- F1-score: 0.769
- Matthews Correlation Coefficient: 0.175
- Confusion Matrix:
     [[65 23]
     [16 13]]

### 15-11-25, 3:10
I tried scaling with StandardScalar but it resulted in sliglty worse metrics so going I would not be proceeding with it going forward 
Metrics
- Accuracy: 0.658
- Precision: 0.816
- Recall: 0.705
- F1-score: 0.756
- Matthews Correlation Coefficient: 0.201
- Confusion Matrix:
     [[62 26]
     [14 15]]


### 15-11-25. 17:30
Added 10 fold cross validation, there was a drop in precision but a rise in the MCC, change in other metrics were marginal at best 
Metrics
- Average accuracy: 0.649
- Average precision: 0.739
- Average recall: 0.785
- Average f1: 0.759
- Average mcc: 0.103

Next Id be Implementing SMOTE

### 16-11-25. 11:14 
I initially augmented the imbalanced dataset using SMOTE to equalize the number of samples for both outcomes — disease present and disease absent — bringing each class to 415 samples. This led to an increase of at least 5% in all evaluation metrics except recall, which dropped from 0.7 to 0.6. Conversely, the Matthews Correlation Coefficient (MCC) showed the greatest improvement, increasing from 0.1 to 0.5.
At this point, although accuracy, recall, precision, and F1-score were above average, they were still lower than the results reported in the original paper.

After progressively increasing SMOTE oversampling up to 4×, performance improved substantially (accuracy 95%, MCC 0.90) with only a 4% train-validation MCC gap, suggesting limited overfitting; however, because SMOTE produces synthetic interpolated samples, I will validate performance on a completely external dataset to ensure true generalizability..

I also included MCC as one of the evaluation metrics — although it was mentioned in the paper, it was not originally used — because it incorporates all values of the confusion matrix (TP, FP, TN, FN) and provides a more reliable measure of performance on imbalanced datasets.

Next: looking into augmentation with CTGAN

### 16-11-25. 17:26
Implemended CTGAN with variying results. Each time the model is fit to the data, it generates differnet data so, so reproducability is low. 

### 16-11-25. 18:46

| Case    | Accuracy | Recall | Precision | F1 Score | MCC    |
| ------- | -------- | ------ | --------- | -------- | ------ |
| BAL-AUG | 0.7169   | 0.8661 | 0.7793    | 0.8192   | 0.1739 |
| DD-AUG  | 0.7783   | 0.9203 | 0.8278    | 0.8696   | 0.0884 |
| TD-AUG  | 0.8096   | 0.9417 | 0.8481    | 0.8912   | 0.1183 |
| QD-AUG  | 0.8090   | 0.9465 | 0.8453    | 0.8916   | 0.0592 |


I was able to specify the seeds,so the results are now more consistent. 
Augmenting the data with CTGAN showed promising results with accuracy, recall, precision, f1 and mcc at 0.8090, 0.9465,	0.8453,	0.8916, and	0.0592 respectively and while good they still below the outcomes seen in SMOTE augmentation. 
A worrisome metric is mcc which is significatly low at 5% and especially when compared to the values of the other metrics indicating that there the datasets may be inbalanced. 

--------
Upon inspection, the GAN-generated dataset was found to be highly imbalanced, with 2781 samples of class 1 and 539 of class 2.

------
I explored several approaches, such as oversampling the minority class and then augmenting with a GAN, or using a GAN followed by random undersampling of the majority class. Unfortunately, all of these strategies performed poorly.

I also considered using SMOTE to balance the dataset before generating new data with a GAN. While SMOTE initially improved performance, the Matthews Correlation Coefficient (MCC) later dropped from 50% to 10%, and the other metrics remained low, as shown in the table below:

| Case    | Accuracy | Recall | Precision | F1 Score | MCC    |
| ------- | -------- | ------ | --------- | -------- | ------ |
| BAL-AUG | 0.7506   | 0.6189 | 0.8472    | 0.7061   | 0.5250 |
| DD-AUG  | 0.6590   | 0.7166 | 0.7396    | 0.7166   | 0.2892 |
| TD-AUG  | 0.6426   | 0.7380 | 0.7159    | 0.7184   | 0.2319 |
| QD-AUG  | 0.6307   | 0.7447 | 0.7034    | 0.7179   | 0.1865 |


I realized that the issue stemmed from the GAN itself, which generates data randomly. As a result, some imbalance was inevitable. In the end, I reverted to the original approach, using only the GAN.


### FINAL RESULTS:

**KNN WITH SMOTE**

|   | case       | accuracy | recall | precision | f1     | mcc    |
| - | ---------- | -------- | ------ | --------- | ------ | ------ |
| 0 | UNBALANCED | 0.6493   | 0.7852 | 0.7393    | 0.7589 | 0.1028 |
| 1 | BAL-AUG    | 0.7506   | 0.6189 | 0.8472    | 0.7061 | 0.5250 |
| 2 | DD-AUG     | 0.8813   | 0.8241 | 0.9338    | 0.8729 | 0.7707 |
| 3 | TD-AUG     | 0.9241   | 0.8933 | 0.9538    | 0.9213 | 0.8518 |
| 4 | QD-AUG     | 0.9500   | 0.9265 | 0.9728    | 0.9486 | 0.9018 |




**KNN WITH CTGAN**

|   | case    | accuracy | recall | precision | f1     | mcc    |
| - | ------- | -------- | ------ | --------- | ------ | ------ |
| 0 | BAL-AUG | 0.7169   | 0.8661 | 0.7793    | 0.8192 | 0.1739 |
| 1 | DD-AUG  | 0.7783   | 0.9203 | 0.8278    | 0.8696 | 0.0884 |
| 2 | TD-AUG  | 0.8096   | 0.9417 | 0.8481    | 0.8912 | 0.1183 |
| 3 | QD-AUG  | 0.8090   | 0.9465 | 0.8453    | 0.8916 | 0.0592 |


### SHAP Explanation

The model’s predictions are driven almost entirely by liver enzyme values.

Key points:

Top drivers: SGPT (ALT), SGOT (AST), and Alkaline Phosphatase are the strongest contributors for both classes.

Moderate influence: Age has a noticeable but smaller effect.

Minimal influence: Bilirubin, proteins, albumin, gender, and A/G ratio contribute very little to the model’s decisions.



### SUMMARY:

Overall Summary

Baseline KNN achieved moderate performance (accuracy ~ 0.65 – 0.67, F1 ~ 0.76) with weak MCC (~ 0.1 – 0.2). Standard scaling did not help.

Cross-validation slightly reduced precision but confirmed stability; metrics largely unchanged.

SMOTE augmentation significantly improved results. Light oversampling gave small gains, while heavier oversampling (up to 4×) produced very strong improvements — reaching 95% accuracy, 0.95 precision, 0.90 MCC, with only mild signs of overfitting.

CTGAN augmentation improved some metrics (accuracy up to ~0.81), but MCC remained very low (0.05–0.17) because the GAN produced heavily imbalanced synthetic data. Attempts to rebalance before/after GAN generation did not solve the problem.

Best performing approach: KNN + high-level SMOTE augmentation, which gave the strongest and most stable metrics:

- Accuracy: 0.95
- Recall: 0.93
- Precision: 0.97
- F1: 0.95 
- MCC: 0.90

SHAP analysis showed the model relies almost entirely on liver enzymes (ALT, AST, ALP), with age as a secondary factor and all other labs contributing minimally.