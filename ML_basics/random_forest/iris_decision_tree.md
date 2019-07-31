## Classification of the Iris Dataset with Decision Tree

Decision Tree can be adopted to perform classification on the Iris dataset. First, import the relevant python modules:
```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib as mpl
from sklearn import tree
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.preprocessing import LabelEncoder
import pydotplus
```

Then we run the main part of the function. First we set the parameters for the **matplotlib** plotting design, such as font and encoding for displaying Mandarin characters.
```
if __name__ == "__main__":
    mpl.rcParams['font.sans-serif'] = ['simHei']
    mpl.rcParams['axes.unicode_minus'] = False
```

Define the features and labels in the Iris data set:
```
    iris_feature_E = 'sepal length', 'sepal width', 'petal      length', 'petal width'
    iris_feature = u'花萼长度', u'花萼宽度', u'花瓣长度', u'花瓣宽度'
    iris_class = 'Iris-setosa', 'Iris-versicolor', 'Iris-virginica'
```

Read the Iris dataset:
```
    path = '..\iris.data' 
    data = pd.read_csv(path, header=None)
```

The first 4 colums are the features listed above. The last column is the label. We read the features and the label from the dataframe loaded above. To enable visualisation, we read only the first two features.
```
    x = data[range(4)]
    y = LabelEncoder().fit_transform(data[4])
    x = x.iloc[:, :2]
```

We use the train test split function from sklearn to generate the training and testing dataset.
```
    x_train, x_test, y_train, y_test = train_test_split(x, y, train_size=0.7, random_state=1)
```

We define the decision tree model that is optimised by the Gini Coefficient. The maximum depth of each tree is defined by 5. The minimum number of points in each class is 10. Then we fit the model.
```
    model = DecisionTreeClassifier(criterion='gini', max_depth=5, min_samples_split=10)
    model.fit(x_train, y_train)
```

Then we calcualte the accuracy by making predictions and calculate the accuracy score:
```
    y_train_pred = model.predict(x_train)   
    y_test_hat = model.predict(x_test)     
    print 'Training set accuracy：', accuracy_score(y_train, y_train_pred)
    print 'Testing set accuracy:', accuracy_score(y_test, y_test_hat)
```

