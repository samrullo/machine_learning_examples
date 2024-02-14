# Holt Winters model

- Adds seasonal components

- There are two ways to combine *trend*, *level* and *seasonal* components

1. Additive : level + trend + seasonal
2. Multiplicative : (level + trend) * seasonal => seasonal grows proportianally with level

# Holt Winters Additive model

- Forecast : $$ y_{t+h|t} = l_{t} + h \cdot b_{t} + s_{t+h-mk} $$

- Level : $$ l_{t} = \alpha \cdot (y_{t} - s_{t-m}) + (1-\alpha)\cdot (l_{t-1} + b_{t-1}) $$

- Trend: $$ b_{t} = \beta \cdot (l_{t} - l_{t-1}) + (1 - \beta) \cdot b_{t-1} $$

- Seasonality : $$ s_{t} = \gamma \cdot (y_{t} - l_{t-1} - b_{t-1}) + (1 - \gamma) \cdot s_{t-m} $$

## Intuition behind t+h-mk

*m* is the period, i.e. after how many time steps does the data repeat itself. In the example of sales, if we say that it is a year and the frequency of timeseries is monthly, then *m* equals 12.



