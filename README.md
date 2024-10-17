# Ethereum Stock Price Prediction

This README provides an overview of the code for predicting Ethereum stock prices using linear regression and autoregressive models. It includes data preparation, model fitting, and visualization steps.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Data Input](#data-input)
3. [Time Series Plot](#time-series-plot)
4. [Modeling](#modeling)
5. [Correlation Analysis](#correlation-analysis)
6. [Autoregressive Model](#autoregressive-model)
7. [Model Visualization](#model-visualization)
8. [Conclusion](#conclusion)

## Prerequisites

Make sure you have the necessary R packages installed. This code specifically requires the `car` package for the Durbin-Watson test.

## Data Input

Read the Ethereum price data from a CSV file.

## Time Series Plot

Create a time series plot to visualize Ethereum prices over time.

## Modeling

Fit a simple linear regression model using time as the predictor.

### Residual Plot

Visualize the residuals from the model.

### Durbin-Watson Test

Perform a Durbin-Watson test to check for autocorrelation in the residuals.

## Correlation Analysis

Create lagged terms for the price and calculate correlations.

## Autoregressive Model

Fit an autoregressive model using the first four lagged terms.

### Durbin-Watson Test for Autoregressive Model

Check for autocorrelation in the residuals.

## Model Visualization

Plot the actual vs. fitted values from the autoregressive model.

<img src="https://github.com/RoryQo/Ethereum-Stock-Price-Prediction/blob/main/graph1.jpg" alt="Ethereum Price Prediction" style="width:600px"/>


## Conclusion

This code provides a comprehensive approach to predicting Ethereum stock prices using linear and autoregressive models.

The analysis indicates that the following lagged terms are important for predicting Ethereum prices:
- **Lag 1**: Shows a strong positive correlation and is significant in the autoregressive model.
- **Lag 4**: Exhibits a significant negative coefficient, suggesting an important delayed effect.
- **Lag 2 and Lag 3**: Although not statistically significant, they still contribute to the model's predictive power.

These insights can help refine future forecasting efforts and suggest that incorporating these lags may improve model performance. For further exploration, consider experimenting with different model orders or additional predictors to enhance forecasting accuracy.
