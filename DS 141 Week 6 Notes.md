#DS141 
#DS-Topic-6

# Naive Bayes Classifiers

## Recap of Probability Concepts
### Conditional
$$P(E|F) = \frac{P(EF)}{P(F)}$$
### [[Bayes' Formula]]
- for F1,F2.. Fn mutually exclusive and exhaustive, E is a different event
$$P(F_j | E) = \frac{P(E|F_j)P(F_j)}{\sum_{i=1}^n{P(E|F_i)P(F_i)}}$$
### Independent Events
$$P(EF) = P(E)P(F)$$
### Multiplication Rule
$$P(EF) = P(E|F)P(F)$$
### Conditional Independence
$$P(EF|G) =  P(E|G)P(F|G)$$

## Naive Bayes Classifiers
- they make naive assumptions about the data
- work on the assumptions that all attributes are equally important and independent
## The LaPlace Estimator
- adds a small number to each of the counts in a frequency table (typically 1) as to not cause a zero probability to make the entire probability 0
## Advantages
- simple to implement and rely on very simple maths
- efficient in terms of storage space and computation time
- scales well to large data sets
## Disadvantages
- relies on an assumption that is often not true in practice
- less reliable than the predicted classes
	- better used for ranking outcomes
- not ideal for data sets with many numerical features
	- suited for categorical features
### Spam Filter
$$P(spam|word) = \frac{P(spam word)}{P(word)}$$
#### Building a spam dataset
- Data must be labelled
- Quanitfy attributes
### Difficulties of dealing with text
- unstructured
- "dirty" data
- context matters
- Different languages
### Text Mining
>[!info] process of deriving info from unstructured text data

- Considers attributes like
	- patterns
	- topics
zo

## In R
- seq_len 
	- creates a sequence that starts at 1 and increments in steps of 1 up to and including the finishing value
		- basically range
- rep.int()
	- replicates a given value a specified number of time
	- (2,5) = 2 2 2 2 2