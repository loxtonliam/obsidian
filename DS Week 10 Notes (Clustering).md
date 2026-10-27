#DS141 
> unsupervised process
# Hierachical Clustering
## Dendograms
![[DS Week 10 Notes (Clustering) - dendogram.png | 400]]
- tree like structures to visualise the hierachical relationship between entities in a data set
- x axis - observations
- y axis - distance
- clustering process is bottom up or aggloremative
- merging takes place based on a chosen distance metric

## Linkage functions

### Measuring distance between clusters
- measuring distance is achieved thru **linkage**

### Complete Linkage
> considers the distance between clusters to be the **maximum** distance between pairs of observations in the clusters
- first step is to compute the distances between each pair of observations, regardless of linkage technique
- merge the closest two observations
	- dist table in R can be used to make a table with observation distances

### Single Linkage
> after creating a cluster, make a new observation table and follow the normal clustering process

### Average linkage
> distance between clusters is considered to be the average distance between pairs of observations in the clusters

- choose the smallest avg distance to merge

### choice of linkage
- complete and average are generally preferred over single
	- single often results in extended, trailing clusters

## Other aspects to consider
- choice of distance measure
- linkage type
- if variables should be scaled

## Summary
 1. Begin with each of the n observations in the data set in its own cluster and calculate the pairwise distance between all observations in the data set, therefore $\binom{n}{2}$ distances
 2. two closest observations are merged to form a new cluster, now (n-1) clusters
 3. compute the new pairwise distances between the n-1 clusters, the choice of linkage method determines how the distances between clusters is defined
 4. Repeat until only a single cluster remains

# k-Means clustering
> each cluster is presented by its centroid

## Calculating cluster centroids
> centroid = geometric center of a group of observations

- avg position of all observations in a cluster
- arithmetic mean of all observaitons in a cluster

## Algorithm
1. specify the desired number of clusters, k
2. randomly assigned a number from 1 to k to each observation
3. for each of the k clusters, calculate the centroid
4. assign each observation in the set to the cluster whose centroid is closest to that observation
5. recalculate each cluster centroid and repeat until cluster assignments do not change

## Issues
### number of clusters
- the choice of k is important
- try a few different values
### Initial cluster centroids
- results are highly dependent on the initial starting config
- recommended to run the algorithm multiple times, with different configs

### Determining best cluster solution
>within cluster variation = sum of squared distances between observations in the cluster and the cluster centroid

- total within cluster variation is the sum of all within cluster variations of each cluster
- best solution has the lowest total within cluster variation