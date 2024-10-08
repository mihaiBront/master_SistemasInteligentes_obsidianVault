---
Subject: Big Data Analytics
Teacher: "@RafaelBerlangaLlavori @RafaelBerlangaLlavori"
Topic: 
date: 2024-10-02
tags:
  - "#SJK006-BigDataAnalytics"
---
# 6.1 Context
![[Pasted image 20241002181235.png]]

Permits analysing any type of indicator or measure, such as:
- Anomaly detection
- Breakage detection
- Stock prediction
- Sales prediction
- Demand
- ...

In a datachain:
![[Pasted image 20241002181354.png]]

Axis:
![[Pasted image 20241002181420.png]]

- X AXIS:
	- Time series
	- Data is sampled in equidistant intervals, to ease the data analysis
- Y AXIS: Value of the variable

> [!note]
> Although, usually, and for the examples in this section, the plots are uni-variant, it can be multi-variant

# 6.2 Components of the time series

- Trend: "*tendencia* " -> Linear regression
- Cyclical Patterns -> 
- Seasonal Patterns -> Seasonal sales, for example, like Christmas
- Irregularities -> Might be interest points
- Noise

Types of the series depending on its characteristics

- Stationary vs NonStationary: Depending on weather the average is constant or not (respectively)
- Multiplicative vs Additive: Depending on weather the standard deviation is NOT CONSTANT or CONSTANT respectively
- Stochastic vs Deterministic (deterministic if there is a function that can exactly represent the progression; stochastic if not)

![[Pasted image 20241002182107.png]]
## How to Normalize a Series:

1. Interpolate missing values
2. Remove trend (slope)
3. Remove seasonality (de-seasonalize) -> Senoidales?

Types of smoothing:
- Basic smoothing:
	- Rolling mean (moving average)
	- Exponential smoothing (helps smoothing the multiplicative character)
- Logarithmic transform
- Differentiation (difference with the shifted series)

>[!abstract]
> ![[Pasted image 20241002183503.png]]
>![[Pasted image 20241002183517.png]]

# 6.3 Predictions

$X = [x_1, x_2 ..., x_n]$
$Y = [y_1, y_2 ..., y_n]$
$\hat{Y} = [\hat{y}_1, \hat{y}_2 ..., \hat{y}_n]$

- X represents a random variable whose values are observed over time (train) 
- Y represents the true values of the time series for the test 
- Ŷ represents the predicted values for the test 
- The model M learns patterns from X to predict Ŷ 
- Comparing Y and Ŷ (errors) gives us the quality of the model (MAE, RMSE, etc.)

![[Pasted image 20241007181822.png]]
![[Pasted image 20241007182008.png]]

Donde, en los modelos tradicionales:
- $p$ es la ventana de predicción
- $q$ es la ventana de suavizado
- $d$ es el intervalo de diferenciación

*Esto funciona con el baseline, el modelo que da los resultados minimamente buenos, si es peor que esto...malament*

En el **machine learning** -> Modelos en su version regresora;
- Features (dimensiones) -> parametros como p, q, d
- <s>Class</s> Regression -> Prediction

