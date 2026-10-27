#DS141 
#DS-Topic-8

# Regression
> the aim is to develop a mathematical model that accurately captures the relationship between the target variable that is a part of real life

## Simple Linear Regression
### Terminology and Notation
> assume a linear relationship between two variables $x$ and $y$
> where $y = f(x) + \epsilon$  

- $f$ is some fixed but unknown function of $x$ 
- $\epsilon$ is some random error term
- We are assuming that 
$$f(x) = B_0 + B_1x$$
- $B_0$ and $B_1x$ are the **parameters** of the model
- we use estimated parameters to reach the **estimated regression model**
$$\hat{y} = \hat{B_0} + \hat{B_1}x$$
- $\hat{y}$ is the predicted value of the [[target variable]]
- difference between predicted and actual value of y is:
$$\epsilon = y - \hat{y}$$
- Residual Sum of Squares  (RSS) is the sum of every points $\epsilon^2$
### Finding least squares estimates of parameters
**derivation**
* consider a sample of n data points $(x_i,y_i), i = 1,2,...,n$ from which the estimates of the paramaters need to be found
* $\epsilon_i = y_i - \hat{y_i}$
* Therefore
$$RSS = \sum^n_{i=1}\epsilon_i^2 = \sum^{n}_{i=1}(y_i-\hat{y_i})^2 = \sum^n_{i=1}[y_i - (\hat{B_0} + \hat{B_1}x_i)]^2 $$
- if RSS has a min value, it will occur for values that satisfy $\frac{\partial RSS}{\partial \hat{B_0}}$ and  $\frac{\partial RSS}{\partial \hat{B_1}}$ 
**solutions**
$$\hat{B_1} = \frac{\sum^n_{i=1}(x_i - \hat{x})(y_i - \hat{y})}{\sum^n_{i=1}(x_i - \hat{x})^2}$$
$$\hat{B_0} = \bar{y} - \hat{B_1}\bar{x}$$
- Therefore in simple linear regression, the estimates of slope and intercept can be calculated using the means of x and y

### Interpretation of regression coefficients
#### Slope coefficient
- sign of slope indicates if the relationship is positive or negative
- 0 slope means there is no linear relationship
#### Intercept
- predicted value of y when x = 0
	- only true when x = 0 within the scope of the data
		- otherwise the intercept is not of interest
### Using a linear regression model for prediction
- just sub in values into the linear equation

### Model Assumptions
1. mean of the probability distribution of $\epsilon$ is 0
2. variance of probability distribution of $\epsilon$ is constant for all settings of the independent variable $x$
3. the probability distribution of $\epsilon$ is normal
4. The errors associated with any two different observations are independent

## Multiple Regression
> contains more than one independent variable

### General form
$$y = B_0 + B_1x_1 + ... + B_px_p + \epsilon$$
- if there are p independent variables
- getting the least squares regression requres p+1 simultaneous equations
	- done in R
