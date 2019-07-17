## What is AUC and ROC?

The terms AUC and ROC frequently appear in machine learning papers but are hardly defined because the concept involved is very basic. Here we briefly define what ROC and AUC are, for machine learning starters.

First, we take a look at the confusion matrix:
![Image](https://www.google.com/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&ved=2ahUKEwjtr8_Xi7zjAhWEA2MBHSOLBnYQjRx6BAgBEAQ&url=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-confusion-matrix-a9ad42dcfd62&psig=AOvVaw2OGDOJD-Uk3u7DOY3-FkCW&ust=1563457629430414)

By comparing the predicted value with the true value, we obtain True Positives, True Negatives, False Positives and False Negatives. 

Then, we can define the following:
TPR (True Positive Rate) = TP / (TP+FN)
FPR (False Postive Rate) = FP / (FP+TN)

We plot TPR against FPR to obtain a curve. This is the ROC (Receiver Operating Characteristic) curve. The area under the curve is called AUC (Area Under Curve). The larger the AUC, the better the classification. This can be illustrated in the example below:

### Example: AUC=1
From definition, if all values are predicted to be True, we have TPR = 1 since FN = 0, and TP = 1. We also have FPR = 1 since FP = 1 and TN = 0. Thus, the ROC curve passes through (1,1).
If all values are predicted to be False, similarly we have TPR = 0 since FN = 1 and TP = 0. We also have FPR = 0 since FP = 0 and TN = 1. Thus, the ROC curve passes through (0,0).
Suppose we have a classifier that correctly classifies all values. Thus, TP = 1, TN = 1, FP =0 and FN = 0. Thus, TPR = 1 and FPR = 0. The ROC curve passes through (0,1). 
The complete curve will generate a square area with side length 1, giving AUC = 1. This represents that all values are correctly classified with this classifier.
