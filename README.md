# Stock-Prediction
Linear regression can be a powerful technique to predict one set of variables knowing the second set of varibles. 
In this project I tried to use regression to predict the price of a given stock at a future date.
Linear regression is commonly used in stock price prediction and even though the predicted price wont be acurate it should predict a close enough value.

The past stock price was imported using quandl and implmenting a dataframe.

```
df = quandl.get('WIKI/GOOGL')
```

After importing the past stock prices the useful data is sorted into closing price , percent change, and volume.

```
df = df[ ['Adj. Open','Adj. High','Adj. Low','Adj. Close','Adj. Volume',]]

df['HL_PCT'] = (df['Adj. High']-df['Adj. Close'])/ df['Adj. Close']*100.0

df['PCT_change'] = (df['Adj. Close']-df['Adj. Open'])/ df['Adj. Open']*100.0

df = df[['Adj. Close','HL_PCT','PCT_change','Adj. Volume']]
```

In this section the features were defined to be past price values and the labels are future price values.
```
forcast_col = 'Adj. Close'

df.fillna(-99999, inplace=True)

forcast_out = int(math.ceil(0.01*len(df)))

df['label'] = df[forcast_col].shift(-forcast_out)
```

Now the data set is trained and tested 

```
X_train, X_test, y_train, y_test = cross_validation.train_test_split(X, y, test_size = 0.2)
```
The above code trains the set of features and tests the set of features and trains thr set of labels and tests the set of labels.

In the below code we have fitted the trained features and trained labels
```
clf = LinearRegression(n_jobs=-1)
clf.fit(X_train, y_train)
accuracy = clf.score(X_test,y_test)
print('Accuracy is',accuracy)
```
Now that the classifier is trained the accuracy is around 97%.


Now that our clasifier is trained we must forcast the price of the stock 
```
forcast_set = clf.predict(X_lately)
print(forcast_set, accuracy, forcast_out
```
After prdicting the stock price the values need to be charted on a graph. This is done by adding the forcasted values to the existing dataframe.

```
for i in forcast_set:
    next_date = datetime.datetime.fromtimestamp(next_unix)
    next_unix += one_day
    df.loc[next_date] = [np.nan for _ in range(len(df.columns)-1)] + [i]
```
Now using the following code the predicted values can be plotted along with the exisiting data frame.
```
df['Adj. Close'].plot()
df['Forecast'].plot()
plt.legend(loc=4)
plt.xlabel('Date')
plt.ylabel('Price')
plt.show()
```
