## Machine Learning for Finance Notes 1
### Basics
Determine the percentage change in price and plot a histogram
```
amd_df['10d_close_pct'] = amd_df['Adj_Close'].pct_change(10)
amd_df['10d_close_pct'].plot.hist(bins=50)
plt.show()
```
To shift data:
```
amd_df['10d_future_close'] = amd_df['Ajd_Close'].shift(-10)
amd_df['10d_future_close_pct'] = amd_df['10d_future_close'].pct_change(10)
```
To find correlations:
```
corr = amd_df.corr()
print(corr)
```

RSI is relative strength index:
- RSI = 100 - 100/(1+RS)
- RS = Average gain over n periods/Average loss over n periods
SMA is smooth moving average. Both SMA and RSI can be calculated using talib. tablib only work on pandas, not numpy arrays.
```
import talib
amd_df['ma200'] = talib.SMA(amd_df['Adj_Close'].values, timeperiod=200)
amd_df['rsi200'] = talib.RSI(amd_df['Adj_Close'].values, timeperiod=200)
```
To plot correlation graphs:
```
import seaborn as sns
corr = feature_target_df.corr()
sns.heatmap(corr, annot=True)
```
Values with 0.2 and above means there is positive correlation.

### Linear Model
```
import statsmodel.api as sm

# create a column of 1s as y-intercept initial values
linear_features = sm.add_constant(features)

# train test split
train_size = int(0.85 * targets.shape[0])
train_features = linear_features[:train_size]
train_targets = targets[:train_size]
test_features = linear_features[train_size:]
test_targets = targets[train_size:]

# linear model
model = sm.OLS(train_targets, train_features)
results = model.fit()
print(results.summary())

print(results.pvalues)
```
p-values test whether the coefficients are significantly different to 0. p-values below 0.05 has cofficents significantly different to 0.
```
# To make predictions:
train_predictions = results.predict(train_features)
test_predictions = results.predict(test_features)
```
To plot results:
```
# Scatter the predictions vs the targets with 80% transparency
plt.scatter(train_predictions, train_targets, alpha=0.2, color='b', label='train')
plt.scatter(test_predictions, test_targets, alpha=0.2, color='r', label='test')

# Plot the perfect prediction line
xmin, xmax = plt.xlim()
plt.plot(np.arange(xmin, xmax, 0.01), np.arange(xmin, xmax, 0.01), c='k')

# Set the axis labels and show the plot
plt.xlabel('predictions')
plt.ylabel('actual')
plt.legend()  # show the legend
plt.show()
```

### Engineering Features
To add nonlinear terms:
```
SMAxRSI = amd_df['14-day SMA'] * amd_df['14-day RSI']
```
To add volume features:
```
amd_df['Adj_Volume_1d_change'] = amd_df['Adj_Volume'].pct_change()
one_day_change = amd_df['Adj_Volume_1d_change'].values
amd_df['Adj_Volume_1d_change_SMA'] = talib.SMA(one_day_change, timeperiod=10)
```
To engineer features using day of the week:
```
print(amd_df.index.dayofweek)
```
The above generates week days as ordinal numbers. To use one-hot representations:
```
days_of_week = pd.get_dummies(amd_df.index.dayofweek, prefix='weekday', drop_first=True)

# Set the index as the original dataframe index for merging
days_of_week.index = lng_df.index

# Join the dataframe with the days of week dataframe
lng_df = pd.concat([lng_df, days_of_week], axis=1)
```
drop_first function removes one column in the dummy variable as we can infer the day of week from the other columns.

### Decision Tree
To build a decison tree:
```
from sklearn.tree import DecisionTreeRegressor
decision_tree = DecisionTreeRegressor(max_depth=5)
decision_tree.fit(train_features, train_targets)
```
To evaluate model, the following outputs R-squared values:
```
print(decision_tree.score(train_features, train_targets))
print(decision_tree.score(test_features, test_targets))
```
To check performance:
```
train_preidictions = decision_tree.predict(train_features)
test_predictions = decision_tree.predict(test_features)
plt.scatter(train_predictions, train_targets, label='train')
plt.scatter(test_predictions, test_targets, label='test')
plt.legend()
plt.show()
```
To try different depth with for loop:
```
# Loop through a few different max depths and check the performance
for d in [3,5,10]:
    # Create the tree and fit it
    decision_tree = DecisionTreeRegressor(max_depth=d)
    decision_tree.fit(train_features, train_targets)

    # Print out the scores on train and test
    print('max_depth=', str(d))
    print(decision_tree.score(train_features,train_targets))
    print(decision_tree.score(test_features, test_targets), '\n')
```

### Random Forest
```
from sklearn.ensemble import RandomForestRegressor
random_forest = RandomForestRegressor()
random_forest.fit(train_features, train_targets)
print(random_forest.score(train_features, train_targets))
```
To set hyperparameters:
```
random_forest = RandomForestRegressor(n_estimators=200, max_depth=5, max_features=4, random_state=42)
```
To set parameter grid search:
```
from sklearn.model_selection import ParameterGrid
grid = {'n_estimators':[200], 'max_depth':[3,5], 'max_features':[4,8]}
from pprint import pprint
ppprint(list(ParameterGrid(grid)))
```
```
test_scores = []

# loop through the parameter grid, set hyperparameters, save the scores
for g in ParameterGrid(grid):
    rfr.set_params(**g)  # ** is "unpacking" the dictionary
    rfr.fit(train_features, train_targets)
    test_scores.append(rfr.score(test_features, test_targets))
# find best hyperparameters from the test score and print
best_idx = np.argmax(test_scores)
print(test_scores[best_idx])
print(ParameterGrid(grid)[best_idx])
```

To extract feature importance:
```
from sklearn.ensemble import RandomForestRegressor

random_forest = RandomForestRegressor()
random_forest.fit(train_features, train_targets)

feature_importances = random_forest.feature_importances_

print(feature_importances)
```
