## Machine Learning for Finance Notes 2
### Scaling data and KNN Regression
Scaling options:
- min-max
- standardization
- median-MAD
- map to arbitrary function (eg. sigmoid, tanh)

sklearn's scaler:
```
from sklearn.preprocessing import scaler
sc = scaler()
scaled_train_features = sc.fit_transform(train_features)
scaled_test_features = sc.transform(test_features)
```
Making subplots:
```
f, ax = plt.subplots(nrows=2, ncols=1)
train_features.iloc[:,2].hist(ax=ax[0])
ax[1].hist(scaled_train_features[:,2])
plt.show()
```
KNN model with parameter selection:
```
from sklearn.neighbors import KNeighborsRegressor

for n in range(2,13):
    # Create and fit the KNN model
    knn = KNeighborsRegressor(n_neighbors=n)
    
    # Fit the model to the training data
    knn.fit(scaled_train_features, train_targets)
    
    # Print number of neighbors and the score to find the best value of n
    print("n_neighbors =", n)
    print('train, test scores')
    print(knn.score(scaled_train_features, train_targets))
    print(knn.score(scaled_test_features, test_targets))
    print()  # prints a blank line
```
To run predictions with teh bets parameters:
```
# Create the model with the best-performing n_neighbors of 5
knn = KNeighborsRegressor(n_neighbors=5)

# Fit the model
knn.fit(scaled_train_features, train_targets)

# Get predictions for train and test sets
train_predictions = knn.predict(scaled_train_features)
test_predictions = knn.predict(scaled_test_features)

# Plot the actual vs predicted values
plt.scatter(train_predictions, train_targets, label='train')
plt.scatter(test_predictions, test_targets, label='test')
plt.legend()
plt.show()
```

### Neural Networks using Keras
```
from keras.models import Sequential
from keras.layers import Dense

model = Sequential
model.add(Dense(50, input_dim=scaled_train_features.shape[1], activation='relu')
model.add(Dense(10, activation='relu')
model.add(Dense(1, activation='linear')

model.complile(optimizer='adam', loss='mse')
history = model.fit(scaled_train_features, train_targets, epochs=50)

plt.plot(history.hitory['loss'])
plt.title('loss:' + str(round(history.history['loss'][-1],6)))
plt.xlabel('epoch')
plt.ylabel('loss')
plt.show()

from sklearn.metrics import r2_score
train_preds = model.predict(scaled_train_features)
print(r2_score(train_targets, train_preds))

plt.scatter(train_preds, train_targets)
plt.xlabel('predictions')
plt.ylabel('actual')
plt.show()
```

To use custom defined loss functions (here MSE):
```
import tensorflow as tf

# create custom loss function
def mean_squared_error(y_true, y_pred):
  loss = tf.square(y_true - y_pred)
  return tf.reduce_mean(loss, axis=-1)

# enable use of loss
import keras.losses
keras.losses.mean_sqaured_error = mean_squared_error

# fit model
model.compile(optimizer='adam', loss=mean_squared_error)
history = model.fit(scaled_train_features, train_targets, epochs=50)
```
To judge whether the prediction is in the right direction, we use the following code, where when the product of true value and predicted value generates negative values, we output True, meaning the direction is wrong.
```
tf.less(y_true * y_pred,0)
```
To use penalty function:
```
def sign_penalty(y_true, y_pred):
  penalty = 10
  # tf.where contains three arguments: 1) Bolean matrix, 2) code run when Bolean is True, 3) code run when Bolean is False
  loss = tf.where(tf.less(y_true * y_pred, 0), penaulty * tf.square(y_true - y _pred), tf.square(y_true - y_pred))
  return tf.reduce_mean(loss, axis=1)

keras.losses.sign_penalty = sign_penalty

# when compiling:
model.compile(optimizer='adam', loss=sign_penalty)

# plotting generates a bow-tie shape
plt.scatter(train_preds, train_targets)
plt.show()
```

Neural network options to combat overfitting:
- Decrease number of nodes
- Use L1/L2 regularisation
- Dropout
- Autoencoder architecture
- Early stopping
- Adding noise to data
- Max norm constraints
- Ensembling

To implement dropout:
```
from keras.layers import Dense, Dropout

model = Sequential()
model.add(Dense(500, input_dim=scaled_train_features,shape[1], activation='relu'))
model.add(Dropout(0.5))
model.add(Dense(100, activation='relu')
model.add(Dense(1, activation='linear'))
```
To implement ensembling:
```
test_pred1 = model_1.predict(scaled_test_features)
test_pred2 = model_2.precict(scaled_test_features)

# horizontally stack predictions and take the average across rows
test_preds = np.mean(np.stack((test_pred1, test_pred2)), axis=1)
```

### Modern Portfolio Theory
```
stocks = ['AMD', 'CHK', 'QQQ']
full_df = pd.concat([amd_df, chk_df, qqq_df], axis=1).dropna()
```
To calculate returns:
```
# calculate daily returns of stocks
returns_daily = full_df.pct_change()

# resample the full dataframe to monthly timeframe with first day of each month
monthly_df = full_df.resample('BMS').first()

# calculate monthly returns of the stocks
returns_monthly = monthly_df.pct_change().dropna()
```
To calculate covariances:
```
convariances = {}
for i in returns_monthly.index:
  rtd_idx = returns_daily.index
  mask = (rtd_idx.month == i.month) & (rtd_idx.year == i.year)
  covairances[i] = returns_daily[mask].cov()
```
To generate portfolio weights:
```
for date in convariances.key():
  cov = covariances[date]
  for single_portfolio in range (5000):
    weights = np.random.random(3)
    weights /= np.sum(weights)
```
Calculating returns and volatility:
```
portfolio_returns, portfolio_volatility, portfolio_weights = {}, {}, {}
for date in covariances.keys(): # contains a list fo timestamp of monthly start
  cov = covariances[date] # the covariances of each month
  
  for single_portfolio in range(5000):
    weights = np.random.random(3) # randomly initialise three numbers [0,1) as weights
    weights /= np.sum(weights) # normalise weights such that they add to 1
    
    returns = np.dot(weights, returns_monthly.loc[date])
    volatility = np.sqrt(np.dot(weights.T, np.dot(cov, weights)))
    
    # date is the key of dict and content is initially an empty list then appended with the value 'returns'
    portfolio_returns.setdefault(date, []).append(returns) 
    protfolio_volatility.setdefault(date, []).append(volatility)
    protfolio_weights.setdefault(date, []).append(weights)
```
To plot efficient frontier:
```
date = sorted(convariances.keys())[-1]
plt.scatter(x=portfolio_volatility[date], y=portfolio_returns[date], alpha=0.5)
plt.xlabel('Volatility')
plt.ylabel('Returns')
plt.show()
```

### Sharpe ratios; features and targets
We can select portfolios with highest return or lowest volatility, but a better option is to choose the optimal Sharpe ratio.
Sharpe ratio = (portfolio return - risk free return)/(portfoio standard deviation).
Here we assume 0 risk free return.

Getting Sharpe ratios:
```
# empty dict for sharpe ratios and best sharpe indexes by date
sharpe_ratio, max_sharpe_idxs = {}, {}

# loop through dates and get sharpe ratio for each portfolio
for date in portfolio_returns.keys():
  for i, ret in enumerate(portfolio_returns[date]):
    volatility = portfolio_volatility[date][i]
    sharpe_ratio.setdefault(date, []).append(ret / volatility)
  
  # get the index of the best sharpe ratio for each date
  max_sharpe_idxs[date] = np.argmax(sharpe_ratio[date])
```
To create features:
```
# calculate exponentially-weighted moving average of daily returns
ewma_daily = returns_daily.ewm(span=30).mean()

# resample daily returns to first business day of the month
ewma_monthly = ewma_daily.resample('BMS').first()

# shift ewma 1 month forward
ewma_monthly = ewma_monthly.shift(1).dropna()
```
To calculate features and targets:
```
targets, features = [], []

# create features from price history and targets as ideal portfolio
for date, ewma in ewma_monthly.iterrows():
    # get the index of the best sharpe ratio
    best_idx = max_sharpe_idxs[date]
    targets.append(portfolio_weights[date][best_idx])
    features.append(ewma)

targets = np.array(targets)
features = np.array(features)
```
To replot efficient frontier:
```
# latest date
date = sorted(covariances.keys())[-1]
cur_returns = portfolio_returns[date]
cur_volatility = portfolio_volatility[date]

plt.scatter(x=cur_volatility, y=cur_returns, alpha=0.1, color='blue')

best_idx = max_sharpe_idxs[date]

plt.scatter(cur_volatility[best_idx], cur_returns[best_idx], marker='x', color='orange')

plt.xlabel('Volatility')
plt.ylabel('Returns')
plt.show()
```

### Machine learning for MPT
To make train and test sets:
```
# make train and test features
train_size = int(0.8 * features.shape[0])
train_features = features[:train_size]
train_targets = targets[:train_size]

test_features = features[train_size:]
test_targets = targets[train_size:]
```
To fit the model of RF:
```
from sklearn.ensemble import RandomForestRegressor

# fit the model and check scores on train and test
rfr = RandomForestRegressor(n_estimators=300, random_state=42)
rfr.fit(train_features, train_targets)

print(rfr.score(train_features, train_targets))
print(rfr.score(test_features, test_targets))
```
To evaluate model performance:
```
# get predictions from model on train and test
test_predictions = rfr.predict(test_features)

# calculate and plot returns from our RF predictions and the QQQ returns
test_returns = np.sum(returns_monthly.iloc[train_size:] * test_predictions, axis=1)

plt.plot(test_returns, label='algo')
plt.plot(returns_monthly['QQQ'].iloc[train_size:], label='QQQ')
plt.legend()
plt.show()
```
To calculate hypothetical portfolio:
```
cash = 1000
algo_cash = [cash]
for r in test_returns:
    cash *= 1 + r
    algo_cash.append(cash)
# calculate performance for QQQ
cash = 1000  # reset cash amount
qqq_cash = [cash]
for r in returns_monthly['QQQ'].iloc[train_size:]:
    cash *= 1 + r
    qqq_cash.append(cash)

print('algo returns:', (algo_cash[-1] - algo_cash[0]) / algo_cash[0])
print('QQQ returns:', (qqq_cash[-1] - qqq_cash[0]) / qqq_cash[0])
```
To plot the results:
```
plt.plot(algo_cash, label='algo')
plt.plot(qqq_cash, label='QQQ')
plt.ylabel('$')
plt.legend()  # show the legend
plt.show()
```

The current models are toy models. Tools for bigger data:
- Python 3 multiprocessing
- Dask
- Spark
- AWS or other cloud solutions
To get more and better data:
- Quandl.com/EOD (free subset available)
