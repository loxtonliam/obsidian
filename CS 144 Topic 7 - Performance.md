#CS144 

# Binary Logarithms
>[!info] binary logarithm of a number N (lg N) is the number x satisfying $2^x = N$ 

# Empirical Analysis
## Run experiments
- Start with moderate input size N
- Measure and record running time
- Double input size
- Repeat
- Tabulate and plot results
## Data Analysis
- plot on log-log scale
- if points are on a straight line, a power law holds - a curve of form $aN^b$ fits
- exponent $b$ is the slope
- solve for $a$ with data
![[CS 144 Topic 7 - Performance- curve fitting math.png]]![[CS 144 Topic 7 - Performance - log log.png|300]]

# Mathematical Models
## Simplifying calculations
- ~
	- use only the fastest growing term
	- ignore slower growing terms
- Rationale
	- When N is large,ignored terms are negligible
	- When N is small, everything is negligible
- $f(N) ~ g(N)$
	- $f(N)/g(N) -> 1 as N -> \infty$
# Doubling Method
1. Start with moderate size
2. measure and record running time
3. double size
4. repeat 1-3
5. verify that ratios of running times approach $2^b$
6. Predict by extrapolation
## Order of growth
>[!info] If a function $f(N)$ ~ $ag(N)$ we say that $g(N)$ is the order of growth of the function
### Classifications

![[CS 144 Topic 7 - Performance - order of growth.png]]![[CS 144 Topic 7 - Performance - graphs.png]]
- Moore's Law - Computer power increases by roughly a factor of 2 every 2 years
# Resizing arrays and memory
## Arrays
- size = number of items inserted
- Capacity = length of the array
## Python 
### Array

- python uses list data type as a resizing array
- Resize when new array created and items copied over, expensive cost
- Ignorance of costs lead to trouble
### Str
>[!note] internally stores two pieces of info for each str object

- memory reference where characters are stored [[contiguously]]
- length of the string
- string concat results in numerous intermediate objects, incurs cost
## Memory
- must be aware of runnnig time and memory usage of the data types used in our programs
- each object stored on the heap
- local vars stored on the stack