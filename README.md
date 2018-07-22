# Automatic Stock trading bot (WIP)

Inspired by [Gekko](https://github.com/askmike/gekko)

## Overview

Aim of this project is to create a bot which can create a portfolio from a given list of stocks, optimizes the portfolio
by efficiently dividing the assets and then does automatic trading to maximize the profit.

Most tutorials on Internet(and my previous project on same topic) use only historial stock price 
to predict future prices. However in this project, important features are extracted from the data and are used in conjuction 
with historical prices. Due to this, it's performance is significantly better than other projects available on Internet.

## Salient features.

### Single point of control

Almost everything can be controlled by changing the parameters in `config.py`. No need to change anything in core code.

### Designed using S.O.L.I.D. principles

No tight coupling, proper encapsulations and abstractions ensure that code is flexible and easy to extend. 

## Target Audience

### Traders who know Finance but not Machine Learning or Programming

Since almost everything is controlled by parameters defined in `config.py`, you don't need to be a programming and/or Machine Learning wizard to predict future stock prices. If you know how to create lists and dictionaries in python, you can easily predict the future.

Currently, data is extracted using **quandl** API. It provides following information about the given stock.
    
    1. Date
    2. Open
    3. High
    4. Low
    5. Close
    6. Adjusted values of all above features
    7. Volume

***trader*** uses Adjusted Close`(Adj_Close)` and Volume as direct historical features to be used for forecasting. If you want to use any other feature(s), change the elements of `REL_DATA_COLUMNS`. For example, if you want to use `Open` and `High`, set `REL_DATA_COLUMNS=['Open', 'High']`. 

Apart from this, ***trader*** contains implementations to extract the following features from the data
  
    1. Simple Moving Average. -> sma
    2. Exponential Moving Average. -> ema
    3. Minimum Bollinger Bound -> min_bollinger_bound
    4. Maximum Bollinger Bound. -> max_bollinger_bound
    5. Daily Returns. -> daily_returns

Above features are defined in `compute_stock_features.py` in `utils` package. In order to use them, change the elements of `REL_PREDEFINED_FEATURES` list defined in `config.py`. For example, if you want to use Simple Moving Average and Daily Returns, set `REL_PREDEFINED_FEATURES = ['sma', 'daily_returns']`.

Summarizing above points, if you want to use Adjusted Close, Volume, Simple Moving Average and Daily Returns for forecasting, set `REL_DATA_COLUMNS=['Adj_Close', 'Volume']` and set `REL_PREDEFINED_FEATURES = ['sma', 'daily_returns']`.

Ultimately, if you want to implement a feature of your own, you can do that too. Although, you will be needing a little knowledge of programming for that. Let's say that you want to add Rolling standard deviation as a feature. To do this, create a python file(say `my_features.py`) preferably in project root directory and define a method(say `get_rolling_std`). After you have done this, open `config.py` and modify `EXTRA_FEATURES` dictionary as follows: set the key to the name of feature and value to the function pointer. In this case, `EXTRA_FEATURES = {'roll_std': get_rolling_std}`. You can have multiple extra features like  `EXTRA_FEATURES = {'roll_std': get_rolling_std, 'another_feature': feature_2_pointer}`. **Only restriction you'll have to follow here is your function should accept only two parameters, first one being the dataframe containing all the data and second one being the column(s) over which you want to use to calculate the feature.** For above example, `get rolling std` will look like

```
def get_rolling_std(data: pd.DataFrame, feature)
    return data[feature].rolling(5).std()
```

Documentation of all parameters will be added soon.
<!---
found [here](https://github.com/Prakash2403/trader/blob/master/config_doc.md)
-->

### Engineers who know Machine Learning but not Finance:

### Software Developers: 

Well, it took me some time to design the UML for this project. I tried to stick to 
**[S.O.L.I.D.](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa)** 
principles as much as possible. I hope it may help you in case you face any design issues. Although I am not an expert in this area, so if you think that there is scope for improvement, feel free to open an issue. 

## Current Status

Future price predictor and portfolio optimizer are ready independently. For predicting future price, following features have
been used
  
    1. Adjusted Close (Directly available from dataset)
    2. Volume (Directly available from dataset)
    3. Simple moving average (Computed explicitly)
    4. Daily Returns (Computed explicitly)
 
