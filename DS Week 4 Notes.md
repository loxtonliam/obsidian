#DS141 
#DS-Topic-4
# Models
>[!info] a representation of some aspect of reality, based on an underlying set of assumptions

- statistical learning model
	- formal mathematical representation used to explain patterns and relationships in data

# [[Supervised Learning]]

- there is a set of data labelled with 'correct' answers to learn from
- a [[target variable]] is used
- the other attributes in the data set are used to estimate the value of this [[target variable]]
# [[Unsupervised Learning]]
- there are no labels or [[target variable|target variables]] to learn from
- the aim is to learn relationships and structure from the data itself

# Basic Types of Models
## Classification
 - aim is to predict, for each [[observation]], which of a set of classes this [[observation]] belongs to
 - used for [[Categorical Data|categorical]] [[target variable]]
 - typically mutually exclusive
	 - [[observation]] can belong to a single class only
- two possible classes -> [[binary classification]]
	- more than two -> multi-class classification
- example of [[Supervised Learning]]

## [[Regression]]
- attempts to predict, for each [[observation]], the numerical value of some [[target variable]] for that [[observation]]
- used for a [[Numerical Data|numerical]] [[target variable]]
- **line of best fit**
- example of [[Supervised Learning]]
## Clustering
- attempts to group observations together by their similarity
	- ie: grouping people by similar likes or dislikes
- no [[target variable]]
- example of [[Unsupervised Learning]]
- 

# Statistical Learning, Formally
## Notation
*Formal aim of statistical learning*
- Suppose $n$ observations on a [[target variable]] $Y$ and $p$ attributes $X1, X2, ... , Xp$ have been gathered. $Y$ is therefore a [[vector]] of length $n$ and each $Xi$ is also a vector of length $n$. We generally write $X = (X1, X2, ... , Xp$), so X is an $n *p$ [[data matrix]]
- Assuming there is a relationship between $Y$ and $X$, this relationship could consist of some systematic, mathematical component, which we attempt to capture (the [[signal]]) and also some random irregularity ([[noise]]).
- This can be written in the general form 
$$Y = f(X) + \epsilon{}$$
- where $f$ is some fixed but unknown function of $X1, X2, ... , Xp$ and $\epsilon$ is a random (error) term. 
- $f$ therefore represents the systematic information that $X$ provides about $Y$
- If we can readily obtain information about $X$ but not $Y$, the value of $Y$ can therefore be estimated by using
$$\hat{Y} = \hat{f} (X)$$
- where (^) represents that they are estimates
## Prediction 
- aim is to find a function $\hat{f}$ such that $Y \approx{} \hat{f}(X)$ for any observation
- $\hat{f}$ is sometimes treated as a black box
	- ie: the most important aspect is that it yields accurate predictions for $Y$ not its exact form
## Inference
- We want to understand how $Y$ changes as  function of $X1, X2, ... , Xp$
- Not interest in predictions for $Y$
- Exact form of $\hat{f}$ must be known
## Parametric methods
- an assumption is first made about the shape of $f$
	- such as assuming $f$ to be linear in [[Regression|linear regression]]
- this simplifies the problem
	- but the model will usually not match the true form of $f$
- the model will obtain poor predictions
## Non-Parametric Methods
- don't make any explicit assumptions about the form $f$
- attempt to find an $f$ that gets as close to the data points as possible
- Therefore requiring more data to obtain estimates
- 