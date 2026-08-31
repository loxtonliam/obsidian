#CS144 
#CS-Sorting-Searching

# Sequential Search

![[CS Sorting and Searching - hint.png]]- use == not 'is'
## Mathematical Analysis of Sequential Search
>[!note] $O(N) = N^2$

•A random search hit checks about half of the N strings on the whitelist, on average.
•A random search miss checks all of theN strings on the whitelist, on average.
•Expected order of growth of running time: $N^2$

# Binary Search
- array must be sorted before hand
```python
def _search(key,a,lo,hi): #given sorted array
	if hi <= lo:
		return -1
	mid = (lo+hi)//2 #find middle index
	if key < a[mid]: #if key less than mid val then key in left half
		return _search(key,a,lo,mid) #recursive call on left half
	elif a[mid] < key: #then key in right half
		return _ search(key,a,mid+1,hi) #recursive call on right half
	else:
		return mid #key is the mid value

def search(key,a):
	return _search(key, a, 0, len(a))
```
## Mathematical Analysis
>[!info] O(n) =  [[lg | lg(n)]]

![[CS Sorting and Searching - Binary.png|500]]

# Insertion Sort
## Basic Idea
• (Visualize a vertical list with 0-index at top.)
• Move down through the list.
• Each item bubbles up above the larger ones above it.
• Everything before (above) the current item is in order.
• Everything after (below) the current item is untouched.

## Implementation
```python
def sort(a):
	n = len(a)
	for i in range(1,n):
		j = i
		while (j>0) and (a[j] < a[j-1]): # while(right > left) -> swap
			a[i],a[j] = a[j],a[i] #swap
			j -= 1
```

## Analysis
>[!note] $O(N) = N^2$

