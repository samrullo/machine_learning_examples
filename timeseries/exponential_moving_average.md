# Exponentially Weighted moving average

- Also known as exponential smoothing and a kind of low pass filter
- Very applicable in machine learning, statistics, finance, signal processing

## Short summary

At each time step, exponentially weighted moving average is a weighted average of time step value at time *t* and exponentially moving average at time *t-1*

- Is calculated on the fly, online

```ewm``` will return exponentially weighted moving object with value alpha

```python
ewma = df["GOOG"].ewm(alpha).mean()
```

## Choosing alpha

- Between 0 and 1
- If alpha is 1, we simply copy the latest data. So this is not an average at all
- Consider alpha zero. Then we are copying previous values and not taking any new values into consideration at all.

- Intuitively setting alpha close to 1 means that we give high weight to new values. New values matter more than old values. This will result noisy smoothing which will closely match the original time series.
- More jagged.

- On ther other hand if we set alpha closer to zero, then older values matter more and this will result in a much more smoother forecasting line

## Arithmetic mean

What is wrong with it?

- It involves adding up all data points. What if we have large amount of data points. In fact we could have infinite number of data points. We won't have enough compute resources
- The required compute grows with the number of data points O(t)



# Exponential smoothing (Again?)

- This will bridge the gap between exponential smoothing and Holt-Winters
- A new perspective for exponential smoothing : time series forecasting

- Previously we were just thinking about ways of taking average over the values. This time we think how we build a model, fit it on data and use it to forecast future values

- Mathematically this is the same as before. But philosophically it is different

# New Notation

- We will use **hats** because they are **predictions**

$$ 
\hat{y_{t}} = \alpha \cdot y_{t} + (1 - \alpha) \cdot \hat{y_{t-1}}   
$$

# The Forecasting model
The forecast for y hat t+1 given data at t
$$ \hat{y_{t+1|t}} = \alpha \cdot y_{t} + (1 - \alpha) \cdot \hat{y_{t|t-1}} $$

Time indices have changed

next prediction = alpha * current value + (1-alpha) * current_prediction

Before : current prediction = alpha * current value + (1-alpha)*last prediction


# Component form

Forecast : $$ \hat{y_{t+h | t}} = l_{t} for h=1,2,3... $$
$$ l_{t} = \alpha \cdot y_{t} + (1-\alpha) \cdot l_{t-1}  $$

$$ l_{t}$$ is called the level

## In code

- Previously used pandas, this time statsmodels
- Can make forecasts, works more like a traditional "model"

```python
from statsmodels.tsa.holtwinters import SimpleExpSmoothing

# make the model. data is univariate
# unlike scikit-learn we pass univariate data not two dimensional with rows and features
# also in scikit-learn we use fit method, here we pass data when initializing
ses = SimpleExpSmoothing(data)

# fit to data. Returns HoltWintersResult object
# this time we won't optimize alpha to minimize forecasting errors
result = ses.fit(smoothing_level = alpha, optimized = False)

# in sample prediction or out of sample forecast
result.predict(start=start_date, end=end_date)
```

## Simpler way to predict

```python
# get all in-sample predictions
result.fittedvalues

# forecast n steps ahead
result.forecast(n)
```

# Holt's Linear Trend Model

- With simple exponential smoothing model, our forecast was always a straight horizontal line
- Holt's Linear Trend Model allows for trends ( lines at any angle )

$$ y = m \cdot x + b $$
$$ y_{t} = m \cdot t + y_{0} $$