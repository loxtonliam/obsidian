#DS141 
#DS-Topic-3

# EDA
>[!info] Exploratory Data Analysis

- useful for identifying any potential problems or issues with data
- part of 'Data Understanding' in the [[CRISP-DM]] process
- basical tools -> visual and numerical summaries of data

# Numerical Summeries of Data

## Parameters and Statistics
- Parmeters - numerical measure that describes a characteristic of a [[population]]
- Statistic - numerical measure that describes a charcteristic of a [[sample]]

## Measures of central tendency
### Mean
> [!info] often referred to as average

$$\bar{x} =\frac{\sum_{i=1}^{n}}{n}$$

- n is no. of observations in the sample
- $\bar{x}$ can also be $\mu$
### Median
> [!info] middle value of a variable

### Mode
> [!info] most frequently occuring value for a particular variable

- denoted as $\bar{x}_{M}$

## Measures of Dispersion

### Range
>[!info] difference between maximum and minimum values of a variable

$$R = x_{(n)} - x_{(1)}$$
### IQR
- before we can define IQR we must define percentiles
- Q1 = 25th percentile
- Q3 = 75th percentile
- IQR = Q3 -Q1

#### Percentiles
- the p'th percentile divided a set into two parts
	- ~ p% of the set have values less than the pth percentile 
	- ~(100-p)% of set have values greater than the pth percentile
##### Calculate Pth percentile
- arrange data in ascending order
$$L_{p} = (n+1)\frac{p}{100}$$
- n = sample size
- $L_{p}$
	- if integer then = location of pth percentile
	- else = interpolate between two observations to approximate the desired percentile

### Variance + Std Dev
- for sample variance $s^2$
$$s^2 = \frac{\sum{(x_i - \bar{x})}^2}{n-1}$$
	- manually
	- $$s^2 = \frac{\sum{x_i^2}}{n-1}-\frac{n\bar{x^2}}{n-1}$$
	- n-1 is used as denominator
- for population variance $\sigma^2$
$$\sigma^2 = \frac{\sum{(x_i - \mu)}^2}{N}$$
	- manually
	- $$\sigma^2 = \frac{\sum{x_i^2}}{N} - \mu^2$$
std-dev = square root of variance
- denoted by either $s$ or $\sigma$

## Measures of association
>[!info] numerical measures that quantify the existence and/or strength of a particular relationship between two variables
### Covariance
>[!info] reveals the direction of the linear relationship between two variables

Given two variables x and y
- sample covariance $s_{xy}$
$$s_{xy} = \frac{\sum{(x_i-\bar{x})(y_i-\bar{y})}}{n-1}$$
- population covariance $\sigma_{xy}$
$$\sigma_{xy} = \frac{\sum{(x_i-\mu_{x})(y_i-\mu_{y})}}{N}$$
- manual calculation
$$s_{xy} = \frac{1}{n-1}[\sum_{i=1}^n{x_iy_i}-\frac{\sum_{i=1}^n{x_i}\sum_{i=1}^n{y_i}}{n}]$$
- > 0 
	- indicates a positive linear relationship
	- if x is above its mean, then y tends to be above its mean 
- < 0
	- negative linear relationship
	- if x is above its mean, then y tends to be below its mean
- 0
	- no linear relationship

### Correlation
- describes both the direction and strength of the [[linear relationship]] between x and y
- sample correlation coefficient
$$r_{xy} = \frac{s_{xy}}{s_xs_y}$$
- population correlation coefficient
$$P_{xy} = \frac{\sigma_{xy}}{\sigma_{x}\sigma_{y}}$$
- the coefficient has no unit
	- falls between -1 and 1
### Outliers
>[!info] item of data that is significantly different from the rest of the data

#### Turkey's Rule
$$outlier > 1.5IQR + Q3 $$
$$outlier < Q1 - 1.5IQR$$
### Missing Values
#### Deletion
- simplest approach to dealing with missing values
- delete all observations with contain missing values in one or more variables
#### Imputation
- we estimate or **impute** these missing values rather than deleting them
### In R
![[R Numerical Summaries.png]]

## Visual Summaries of Data
### Bar Charts
- used to portray frequencies 
- provide an effective way to compare and contrast values for different categorical variables
- ![[Bar Chart.png]]

### Pie Chart
- used for categorical data 
- display [[relative frequencies]]
- ![[Pie Chart.png]]

### Line Chart
- used to show changes in values over times
- x-axis typically shows sequential time values
- ![[line chart.png]]
### Scatterplot
- used to display values of a pair of numerical variables
- useful to uncover trends in data, easy to detect outliers
- ![[scatterplot.png]]
### Boxplots
- also known as box and whisker diagram
- box
	- shows Q1, Q2,Q3
- whiskers
	- show min and max of function
- dots
	- outliers
![[boxplot.png]]

### Histograms
- visualise the distribution of [[numerical data]]
- constrcution begins with a frequency table
![[histograms.png]]

### Data Visualisation in R
- done using the **ggplot2** package
![[ggplot2 package.png]]