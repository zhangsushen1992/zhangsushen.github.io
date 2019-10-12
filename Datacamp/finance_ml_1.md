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
