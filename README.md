<h1 align="center">Ethereum Stock Price Prediction</h1>

<h2 align="center">Table of Contents</h2>

<table align="center">
  <tr>
    <td><a href="#overview">Overview</a></td>
    <td><a href="#statistical-tests">Statistical Tests</a></td>
  </tr>
  <tr>
    <td><a href="#prerequisites">Prerequisites</a></td>
    <td><a href="#visualization">Visualization</a></td>
  </tr>
  <tr>
    <td><a href="#data">Data Description</a></td>
    <td><a href="#conclusion">Conclusion</a></td>
  </tr>
  <tr>
    <td><a href="#modeling">Modeling</a></td>
    <td></td>
  </tr>
</table>


## Overview

The goal of this project is to predict Ethereum stock prices by analyzing historical price data. The data is processed and used to fit a linear regression model with time as the only predictor, and an autoregressive model that incorporates lagged values of the price. The effectiveness of the models is evaluated by residual analysis, correlation analysis, and statistical testing.

### Key Features:
- Time series visualization to observe Ethereum's price trend.
- Linear regression model to predict Ethereum prices based on time.
- Autoregressive (AR) model to incorporate lagged price terms for prediction.
- Residual analysis and Durbin-Watson test to check for autocorrelation.
- Correlation analysis and autocorrelation tests (ACF, Box Test) for determining significant lags.
- BIC criterion for selecting the optimal number of lags in the autoregressive model.

## Prerequisites

Before running the code, ensure you have the necessary R packages installed. This project uses the following R package:
- `car` – For performing the Durbin-Watson test.
- `stats` – For statistical tests and modeling.

## Data

The Ethereum price data is read from a CSV file containing the price history. The dataset should have at least two columns: `time` (the time index in days) and `price` (the price of Ethereum in dollars).

- **Time**: The time index (in days) starting from a specific date (e.g., January 1st, 2022).
- **Price**: The Ethereum price (in USD) for each corresponding day in the `time` column.

Example data format:
| Time | Price  |
|------|--------|
| 01/01/22    | 3500.12 |
| 01/02/2022    | 3450.67 |
| 01/03/2022    | 3550.90 |
| ...  | ...    |


## Autocorrelation Tests

### Creating Lags

To build the autoregressive model, we first create lagged versions of the price data. Lagged variables are important because they allow the model to account for the effect of previous days' prices on future prices.

The dataset is augmented with lagged terms, where each lag corresponds to a previous day’s price. For example, lag 1 corresponds to the price from the previous day, lag 2 to the price from two days ago, and so on. We create up to 10 lagged terms to consider a range of time-dependent influences.

```r
# Creating lagged terms
ethereum$lag1[2:94] = ethereum$price[1:93]
ethereum$lag2[3:94] = ethereum$price[1:92]
ethereum$lag3[4:94] = ethereum$price[1:91]
# Repeat for lag4 through lag10
```

Once the lagged terms are created, we compute the correlation matrix between the actual price and the lagged prices. This helps us identify which lags have the strongest relationship with the current price, and therefore should be considered in the model.

```
# Correlation matrix for lagged terms
round(cor(ethereum[c(11:94),c(3:13)]), 4)
```


### ACF (Autocorrelation Function)

The autocorrelation function (ACF) is a useful tool for analyzing the relationship between a time series and its lagged values. A significant autocorrelation at a particular lag indicates that past values have predictive power for future values. We visualize the ACF for the Ethereum price data to assess how far back in time the prices influence each other.



## Modeling

### Autoregressive Model

Incorporating lagged values of Ethereum prices into the model, we fit an autoregressive (AR) model. This model takes into account the previous time steps to make predictions about future prices.



## BIC Criteria for Lag Selection

The Bayesian Information Criterion (BIC) is used to select the optimal number of lags in the autoregressive model. The BIC penalizes the inclusion of too many lag terms, thus helping to avoid overfitting.

We compare the BIC for autoregressive models with different numbers of lags (from 1 to 10) and choose the model with the lowest BIC value.

```
# Fit AR models with varying number of lags and calculate BIC
bic_values <- sapply(1:10, function(i) {
  model <- ar.ols(ethereum$price, order.max = i, demean = F, intercept = T)
  BIC(model)
})

# Choose the optimal number of lags based on the lowest BIC value
best_lag <- which.min(bic_values)
```

## Statistical Tests

### Durbin-Watson Test

The Durbin-Watson test is used to check for autocorrelation in the residuals of the regression models. Autocorrelation can affect the model's accuracy, and this test helps us assess whether the residuals from our models are independent.

```
# Perform Durbin-Watson test
durbinWatsonTest(model)
```

### Box Test

The Box test can be used to determine whether there is autocorrelation in the residuals from the autoregressive model. If significant autocorrelation exists, it may indicate the need for additional lags or a more complex model.

```
# Perform the Box Test
Box.test(residuals(model), lag = 10, type = "Ljung-Box")
```

## Visualization

### Residual Plots

Residual plots are created to visualize the differences between observed and predicted values for both the linear regression and autoregressive models. These plots help in diagnosing the model fit and detecting any potential issues.

<img src="https://github.com/RoryQo/Ethereum-Stock-Price-Prediction/blob/main/Graph2.jpg" alt="Graph 2" style="width:400px"/>

### Model Fit Visualization

The final step is to visualize how well the fitted models (both linear and autoregressive) match the actual Ethereum prices. The fitted values are plotted alongside the actual prices to compare the model's predictions against reality.

<img src="https://github.com/RoryQo/Ethereum-Stock-Price-Prediction/blob/main/graph1.jpg" alt="Ethereum Price Prediction" style="width:600px"/>

## Conclusion

This project demonstrates the process of predicting Ethereum prices using both simple linear regression and autoregressive models. The key insights from the analysis are:

- The linear regression model provides a basic baseline prediction.
- The autoregressive model, incorporating lagged values of the price, yields a better prediction by taking into account the historical price trend.
- The Durbin-Watson test and residual analysis help to ensure the models' validity and diagnose issues like autocorrelation.
- The BIC criterion helps to determine the optimal number of lags to use in the autoregressive model.

### Key Findings:
- **Lag 1**: The first lag term is highly significant and shows a strong correlation with future Ethereum prices.
- **Lag 4**: Exhibits a significant delayed effect on price, with a negative coefficient suggesting some market dynamics may take time to manifest.
- **Lags 2 and 3**: Although not statistically significant, they still contribute to the predictive power of the autoregressive model.

### Future Work:
- Experimenting with different model orders and additional predictors, such as external economic indicators or sentiment analysis, could improve the prediction accuracy.
- Exploring more advanced models like ARIMA, GARCH, or machine learning-based approaches (e.g., LSTM) might provide better results.

This approach lays the groundwork for Ethereum price prediction and can be extended for more complex forecasting models.
