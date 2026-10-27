# K-Nearest Neighbours
## Distance Measures
### 2d
![[DS Week 9 Notes - 2d table.png]]![[DS Week 9 Notes - distance.png]]
- distance given by $\sqrt{(40-23)^2 + (10-2)^2}$

### Euclidean Distance
$$d = \sqrt{(a_1-b_1)^2 + (a_2-b_2)^2 + ... + (a_n-b_n)^2}$$
- distance is unitless
	- numbers themselves have no meaningful interpretation

### Manhattan Distance
$$d = |a_1-b_1| + |a_2-b_2| + ... + |a_n-b_n|$$
![[DS Week 9 Notes - manhattan.png]]

### Jaccard Distance
- often used when working with binary data
**Jaccard Index**

$$I = \frac{|A\cap B|}{|A\cup B|}$$
**Jaccard Distance**
$$d = 1 - Jaccard_Index$$
$$d = 1 - \frac{|A\cap B|}{|A\cup B|}$$
### Other Metrics
#### Chebyshev Distance
- also known as maximum distance
$$ d= max_i(|a_i-b_i|)$$
#### Cosine Distance
- often used in text classification 
- used to measure the similarity of two documents

#### Minkowski Distance
- generalised form of Manhattan and Euclidean metrics
#### Canberra Distance 
- weighted version of the Manhattan distance

#### Levenshtein distance
- one of the family of **edit distance** metrics
- used in natural langauge processing
	- measures the differnce between two string sequences
>[!info] minimum number of edits (insertions, deletions or substitutions) required to change on word to another

eg: computer -> compute = 1 (deletion)

### Limitations

#### Categorical Variables
- distance metrics like Euclidean require numerical attirbutes
- Categorical Vairables should be encoded numerically
- You could just assign each group a number
	- but this implicitly gives the categories an order
- Dummy coding is a better opraoch

#### Sensitivity to scale
- values are resacled before the distance between entities are calculated
**min max normalisation**
$$scaled x_i = \frac{x_i - min(x)}{max(x) - min(x)} = \frac{x_i - \bar{x}}{S_x}$$

#### Sensitivity to Outliers
- presence of outliers can strongly influence distance calculations
- outliers should be indentified and removed

#### Curse of deminsionality
- Euclidean distance can be problematic in high-dimensional spaces
- think about reducing amt of attributes

## Nearest Neighbours for Classification
- essentially just use surrounding data points to classift a specific point
- ![[DS Week 9 Notes - neighbours.png]]

### k-NN Algorithm
> [!info] k nearest neighbour

#### Terminology
- k refers to the number of neighbours being considered
- one of the aforementioned distance methods must be used to calculate the distance

#### Choice of k
- k can be any value from 1 to n (n being number of observations)
-  k = n
	- means every observation in the data set get a vote when classification is mode
	- this means the majority class will always be predicted
- k = 1
	- outliers and mislabelled observations might unduly influence classifications
- k = root n is a common starting place

#### Combining votes
- weighted voting can be used,
	- close neighbours contribute more than those that are futher away

#### k-NN and probability estimation
- like a deicsion tree

#### Regression
- if the target variable is numeric, the avg or median target variable could be used to make a prediction for a new observation

### Strengths
- Simplicity
	- easy to use algorithm
- Versatility
	- can be used for classification and regression
- No assumptions
- Fast training

### Weaknesses
- Pre-processing required
- Scalability
	- it requires lots of memory
- Curse of dimensionality
- Prone to overfitting

## Overfitting

### Basic Idea
- captures too much noise
### Guarding against overfitting
- divide the data set into two components
	- training data
	- test data

## Variable Selection
- identify a subsest of data to use in a model