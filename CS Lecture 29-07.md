#CS144
#3-2-Creating-Data-Types

# Object-oriented Programming (OOP)
> [!info] create your own data types

## Impact
- Allows the client to use data types without knowing the implementation

## Steps to create a data type

### 0. Design
- Understand the problem domain by asking ppl
- Draw it out and describe needs

### 1. Design and specify an [[API]]
- Decide on method names and specify what they do
- Provides clients clear understand on how to use the class

### 2. Compose Test Client
- Validate and test the design with test cases
### 3. Implement Python Class that meets [[API]]
example code
```python
class Charge:
	def __init__(self,x0,y0,q0):
		self._rx = x0
		self._ry = r0
		self._q = q0
```
#### [[Constructors]]
- Like a method with special name _ _init_ _
	- self - special parameter var
	- x0 - ordinary parameter var
- Creates instance of object, return identity of object
- Initializes object's instance variables
#### [[Instance Variables]]
- set of type variable
- Belong to particular instance of class
- **self** keyword
	- part of each instance
- the _ before the variable name indicates it should not be accessed by the client 
	- ie: private variable
#### [[Methods]]
- Similar to functions
- Defined within a class
- Direct access to instance variables
> [!warning] An instance variable reference in an instance method **refers to the value for the object that was used to invoke the method**


## Implementation
![[OOP Implementation.png]]

