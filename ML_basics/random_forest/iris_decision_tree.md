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
    iris_feature_E = 'sepal length', 'sepal width', 'petal length', 'petal width'
    iris_feature = u'花萼长度', u'花萼宽度', u'花瓣长度', u'花瓣宽度'
    iris_class = 'Iris-setosa', 'Iris-versicolor', 'Iris-virginica'
```

Read the Iris dataset:
```
    path = '..\iris.data' 
    data = pd.read_csv(path, header=None)
```

The first 4 colums are the features listed above. The last column is the label. We read the features and the label from the dataframe loaded above. The value y is normalised using **LabelEncoder()**. To enable visualisation, we read only the first two features.
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
To save the model, we use **tree.export_graphviz**.
```
    with open ('iris.dot', 'w') as f:
        tree.export_graphviz(model, out_file=f)
```
If the output is in the format .dot:
```
    tree.export_graphviz(model, out_file='iris1.dot')
```
If the output is in pdf format, we have the following code:
```
    dot_data = tree.export_graphviz(model, out_file=None, feature_names=iris_feature_E[0:2], class_names=iris_class, filled=True, rounded=True, special_characters=True)
    graph.write_pdf('iris.pdf')
    f = open('iris.png', 'wb')
    f.write(graph.create_png())
    f.close()
```
We then draw the graph for the 
```
    N, M = 50, 50
    x1_min, x2_min = x.min()
    x1_max, x2_max = x.max()
    t1 = np.linspace(x1_min, x1_max, N)
    t2 = np.linspace(x2_min, x2_max, M)
    x1, x2 = np.meshgrid(t1, t2)
    x_show = np.stack((x1.flat, x2.flat), axis=1)  
    print(x_show.shape)
```
We set the color codes for plotting the graph:
```
    cm_light = mpl.colors.ListedColormap(['#A0FFA0', '#FFA0A0', '#A0A0FF'])
    cm_dark = mpl.colors.ListedColormap(['g', 'r', 'b'])
```
The we input the arbitrary grid values _x_show_ into the model to generate _y_show_hat_.
```
    y_show_hat = model.predict(x_show)  
    print(y_show_hat.shape)
    print(y_show_hat)
```
Then we reshape the output values of _y_show_hat_ to the shape of the input:
```
    y_show_hat = y_show_hat.reshape(x1.shape)
    print(y_show_hat)
```
We generate settings to plot the graph. All the x values are plotted with dots. All the testing x values are plotted with *.
```
    plt.figure(facecolor='w')
    plt.pcolormesh(x1, x2, y_show_hat, cmap=cm_light)  # Predicted values
    plt.scatter(x_test[0], x_test[1], c=y_test.ravel(), edgecolors='k', s=100, zorder=10, cmap=cm_dark, marker='*')  # Testing values
    plt.scatter(x[0], x[1], c=y.ravel(), edgecolors='k', s=20, cmap=cm_dark)  # All data
    plt.xlabel(iris_feature[0], fontsize=13)
    plt.ylabel(iris_feature[1], fontsize=13)
    plt.xlim(x1_min, x1_max)
    plt.ylim(x2_min, x2_max)
    plt.grid(b=True, ls=':', color='#606060')
    plt.title('Decision Tree for the Iris Dataset', fontsize=15)
    plt.show()
```
