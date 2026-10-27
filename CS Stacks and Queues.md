#CS144

## Data Types
- A set of values and the operations defined on those values
- Some are built into Python
- Others are user-created

## Data Structures
- Represent data or relationships among data

**Built into Python:**
- `list`
- `dictionary`
- `tuple`
- `set`
- ...

**Not built into Python:**
- Linked list
- Circular list
- ...

**Constraints when choosing a data structure:**
- Available memory
- Time allowed

## Stacks and Queues APIs

### Collection
An [[Abstract Data Type|ADT]] whose values are a multiset of items, all of the same type.

> Stacks and queues are both fundamental ADT collections.

### Stack

**Operations:**
- Add an item to the collection
- Remove and return the item most recently added (**LIFO**)
- Test if the collection is empty
- Return the size of the collection

**API:**
![[CS Stacks and Queues - Stack API.png]]

**example**
![[CS Stacks and Queues - Example of stack ops.png]]
### Queue

**Operations:**
- Add an item to the collection
- Remove and return the item least recently added (**FIFO**)
- Test if the collection is empty
- Return the size of the collection

**API:**
![[CS Stacks and Queues - Queue API.png]]

**Example**
![[CS Stacks and Queues - Q ops.png]]
### Performance Specs
- all operations are O(1)
- Memory use in linear in the size of the collection, when it is nonempty
- No limits within the code on the collection size

# Sequential vs Linked Data Structures
**Sequential**
- puts objects next to one another
- Machine: consecutive memory cells
- Python: array of objects
- has a fixed size, arbitrary access
**Linked**
- Associate with each object a **link** to another one
- Python: link is a reference to next object
- Variable size, sequential access
## Linked List
- recursive data structure
>[!info] null or a reference to a [[node]]

**Representation**
- Use a private nested class *Node* to implement the node abstraction
- Nodes have two values
	1. item
	2. next *Node*
### Singly-linked data structure

![[CS Stacks and Queues - linked.png]]