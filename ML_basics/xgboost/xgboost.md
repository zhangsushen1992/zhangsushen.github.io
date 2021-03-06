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

The error rate is generated by the following function:
```
def error_rate(y_hat, y):
    return 'error', float(sum(y.get_label() != (y_hat > 0.5))) / len(y_hat)
```

In the main of the code, we first read the data from txt files:
```
if __name__ == "__main__":
    # Read data
    data_train = xgb.DMatrix('agaricus_train.txt')
    data_test = xgb.DMatrix('agaricus_test.txt')
```

Then we define the parameters. Max_depth is the depth of the decision tree. 
```
    param = {'max_depth': 3, 'eta': 0.3, 'silent': 1, 'objective': 'binary:logistic'}
    # param = {'max_depth': 3, 'eta': 0.3, 'silent': 1, 'objective': 'reg:logistic'}
    watchlist = [(data_test, 'eval'), (data_train, 'train')]
    n_round = 6
    bst = xgb.train(param, data_train, num_boost_round=n_round, evals=watchlist)
    # bst = xgb.train(param, data_train, num_boost_round=n_round, evals=watchlist, obj=g_h, feval=error_rate)
```

Then we find the error rate:
```
    y_hat = bst.predict(data_test)
    y = data_test.get_label()
    print y_hat
    
    error = sum(y != (y_hat > 0.5))
    error_rate = float(error) / len(y_hat)
    print 'Total number of samples：\t', len(y_hat)
    print 'Number of errors：\t%4d' % error
    print 'Error rate：\t%.5f%%' % (100*error_rate)
```
