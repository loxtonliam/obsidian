#DS141 
#DS-Topic-5

# Selecting Informative Attributes
- uses the attributes to predict the value for the [[target variable]] as a typical [[Classification|classification model]]
- tree diagrams can be constructed to represent the rules that determine the [[target variable]]
- Groups can be [[recursively partitioned]], resulting in groups that are homogeneous in terms of their values of the [[target variable]]
## [[Entropy|Entropy as a purity measure]]
>[!info] measure of disorder that can be applied to a set
- ie: lower entropy = more pure/ less randomness
it is defined as
$$-p_1log_2(p_1) - ... p_2log_2(p_2) - ... = \sum_{i=1}^c{-p_ilog_2(p_i)}$$
- where $c$ is the number of classes
- $p_i$ is the relative proportion of property $i$ within the set $0 <= p_i <= 1$ 
- Max = $log_2(n)$ where there are $n$ possible classes
-  Can be caluclated using $Entropy()$ function from Desc_Tools

>[!hint] use log base change rule 
>$$log_b(a) = \frac{log_c(a)}{log_c(b)}$$

## [[Information Gain]]
>[!info] measure of change in [[entropy]] due to new information being added

- parent set = original set before segmentation takes place
- children = after segmentation
- The amount of information gain depends on how much purer the children are than the parent set

mathematically
$$IG(parent,children) = entropy(parent) - [p(c_1)*entropy(c_1) + p(c_2)*entropy(c2) + ...]$$
- where IG = information gain

- can be calculated using the $attrEval()$ from the CORELearn package

# Decision Trees for Classification
## Structure of decision trees

- begin with a root at the top, which then splits into narrower branches as it grows downwards
- can be used as [[Classification| classification]] alogrithms
- can be built using [[Entropy]] and [[Information Gain]]
- Top is called the **root node**
	- leaf nodes are known as **terminal nodes**
![[DS 141 Week 5 Notes - decision tree.png|500]]

## Decision Tree Boundaries
![[DS 141 Week 5 Notes - boundaries.png|500]]
- attributes can be used more than once in segmentations
	- Age is split depending on balance
- not all terminal nodes need to be pure
- interior node = decision node

## Important Issues in Decision Tree Classification
### Numeric Attributes
- should pick a threshold point for the numerical value that provides the most [[Information Gain|information gain]]
### Pruning a Decision Tree
- [[recursively partitioned|recursive partitioning]] can theoretical be done until every terminal node is pure
	- may result in the tree being very complex and not [[generalising model|generalising]] well
- How to prevent this
	- the segmentation process can be stopped before the tree becomes too complex
	- **early stopping citerion/pre-pruning**
- 
