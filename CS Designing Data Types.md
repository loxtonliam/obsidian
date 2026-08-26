#CS144 
#3-3-Designing-Data-Types
# Encasuplation
>[!info] process of seperating clients from implementations by hiding information.

- python uses _
- the API should be be the only dependence between client and implementation
-

# Immutability
- values cannot change once created
- List and arrays are mututable
## Advantages
- easier to use
- objects mantain consistent state
## Disadvantages
- Create new object for every value

# Polymorphism
- different types of objects can be accessed through a single,common interface
## Operator Overloading

![[CS Designing Data Types - Operator Overloadimng.png|600]]

# Equality Testing
- Reference equality - holds when two references are equal - they refer to the same object
- Object equality - holds when two objects are equal - same values
- an \_\_eq\_\_ function is defined otherwise **is** function is used
![[CS Designing Data Types - equality testing.png]]
## Hashing
- special method for equality testing
- it maps an object to an integer, known as a **hash code**
- for an object to be hashable it must
	- object can be compared for equality with other objects via the == operator
	- Whenever two objects are equal, they have the same hash code
	- object's hash code does not change throughout it's lifetime
- function -> \_\_hash\_\_
- 
## Comparison Operators
![[CS Designing Data Types - comparison.png]]
# Built in Functions
 ![[CS Designing Data Types - functions.png]]


# Inheritance
- essentially class within another class
```python
class Person:
	def __init__(self, name):
		self._name = name
	def getName(self):
		return self._name
class Employee(Person):
	def __init__(self, name, staffnum):
		Person.__init__(self,name)
		self._staffnum = staffnum
	def getEmployee(self):
		return self.getName()+", "+self._staffnum

x = Person("Marge")
y = Employee("Homer", "1007")

print(x.getName())
print(y.getEmployee())
```
- Employee class utilises the already constructed class of a person to create it's data types
