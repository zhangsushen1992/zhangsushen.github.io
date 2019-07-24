## The Implementation of XGBoost Algorithm

XGBoost Algorithm is a popular gradient boosting algorithm that became popular after its application in the winning case of the Kaggle competition- Higgs Machine Learning Challenge. Developed by Tianqi Chen, it has gained much popularity in recent years.

The application in Python is very simple. First it is required to download the **xgboost** package.
``` 
pip install xgboost
```

Then, we import the relevant packages:
```
import xgboost as xgb
import numpy as np
from sklearn.tree import DecisionTreeClassifier
```

We define the logistic loss function (p), the gradient (g), and the second order derivative (h) in the following function:
```
def g_h(y_hat, y):
    p = 1.0 / (1.0 + np.exp(-y_hat))
    g = p - y.get_label()
    h = p * (1.0-p)
    return g, h
```
