---

## title: 13 - Symbol Tables tags: [computer-science, python, data-structures, symbol-tables, hash-tables, bst] source: Sedgewick, Wayne & Dondero - Intro to 
---
#CS144 
## 1. Motivation

Sorting the whole list every time you add an entry (mergesort + binary search) is painful.

> [!example] Why telephone books are obsolete Unsupported operations:
> 
> - Change the number for a given name
> - Add a new name with a number
> - Remove a name and its number
> 
> Mergesort + binary search has the same problem with add and remove.

**Bottom line:** we need a more flexible API.

### Associative array abstraction

Imagine arrays whose indices are **strings**:

```python
phoneNumbers["Alice"] = "(212) 123-4567"
phoneNumbers["Bob"]   = "(609) 987-6543"
IPaddrs["www.yale.edu"] = "130.132.143.21"
```

- Use **keys** to access associated **values**
- Keys and values can be any type of data
- Client code could not be simpler
- Legal in Python, not in Java

### Applications

|Application|Key|Value|
|---|---|---|
|contacts|name|phone number, address|
|credit card|account number|transaction details|
|file share|name of song|computer ID|
|dictionary|word|definition|
|web search|keyword|list of web pages|
|book index|word|list of page numbers|
|cloud storage|file name|file contents|
|DNS|domain name|IP address|
|reverse DNS|IP address|domain name|
|compiler|variable name|value and type|
|internet routing|destination|best route|

---

## 2. Symbol table ADT

> [!note] Definition A **symbol table** is an ADT whose values are sets of key-value pairs, **with all keys different**.

**Basic operations**

| Operation    | Meaning                                                                                  |
| ------------ | ---------------------------------------------------------------------------------------- |
| **Put**      | Associate a key with a value. If the key is new, add it. If it exists, change its value. |
| **Get**      | Return the value associated with a key.                                                  |
| **Contains** | Test whether a key is in the table.                                                      |
| **Iterate**  | Go through the keys.                                                                     |

**Useful additional assumptions**

- Keys are comparable and iteration is in order
- No limit on the number of key-value pairs

### Benchmark example: word counts

Keys are `str`, values are `int`. Input: `it was the best of times it was the worst`

- Each new word is added with value 1
- A repeat word changes its value (`it` becomes 2, `was` becomes 2, `the` becomes 2)

### Why ordered keys?

- **Natural for many applications:** numbers, strings, dates/times, client-supplied types (colour, length)
- **Enables efficient implementations:** mergesort, binary search, BSTs
- **Enables useful API extensions:** keys in sorted order, find the _k_th smallest key

---

## 3. Python API and iteration

Python uses **operator overloading** and **duck typing** with placeholder type names for keys and values. Other languages use _generics_.

```python
class SymbolTable:
    SymbolTable()                  # create a symbol table        st = SymbolTable()
    __setitem__(key, val)          # put                          st[key] = val
    __getitem__(key)               # get                          val = st[key]
    __contains__(key)              # contains                     key in st
    __iter__()                     # iterate over keys            for key in st:
```

> [!warning] Restrictions on key type
> 
> - Hash table implementation: keys must be **hashable**
> - BST implementation: keys must be **comparable**

**Goal:** simple, safe, clear client code for symbol tables holding any type of data.

### Iteration

`for` loops work on any **iterable** object (`list`, `set`, `tuple`, `dict`, `str`), not just ranges and arrays.

```python
stack = Stack()
...
for s in stack:
    stdio.writeln(s)
```

To make your own collection iterable, reuse a list and return `iter()` of it:

```python
class Stack:
    def __init__(self):
        self._a = []           # items
    def __iter__(self):
        return iter(self._a)
```

- Performance spec: constant time per item
- **Make your collections iterable for easy use by clients**

---

## 4. Client examples

All three use `OrderedSymbolTable` from `bst.py`.

### Example 1: Sort with dedup (`sort.py`)

Goal: sort lines on stdin and remove duplicates. Key is the line (`str`), value is ignored.

```python
import stdio
from bst import OrderedSymbolTable

def main():
    st = OrderedSymbolTable()
    while not stdio.isEmpty():
        line = stdio.readLine()
        st[line] = 0          # add line as key, ignore value
    for key in st:            # iterates in sorted order
        stdio.writeln(key)

if __name__ == '__main__': main()
```

Duplicates disappear because setting an existing key just overwrites its value.

### Example 2: Frequency counter (`freq.py`)

Goal: word frequencies. Key is `str`, value is `int`.

```python
def main():
    st = OrderedSymbolTable()
    while not stdio.isEmpty():
        key = stdio.readString()
        if key in st: st[key] = st[key] + 1
        else:         st[key] = 1
    for key in st:
        value = st[key]
        stdio.writef("%8d  %s\n", value, key)
```

Pipe into `sort.py` to order by count: `python freq.py < tale.txt | python sort.py`

### Example 3: Index (`index.py`)

Goal: print an index of where each word occurs. Key is `str`, value is a `list` of `int` positions.

```python
words = stdio.readAllStrings()
bst = OrderedSymbolTable()
for i in range(len(words)):
    word = words[i]
    if not word in bst:
        bst[word] = []
    bst[word] += [i]

for word in bst:
    occurrences = bst[word]
    stdio.write(word + ': ')
    for occurrence in occurrences:
        stdio.write(str(occurrence) + ' ')
    stdio.writeln()
```

Sample output: `it 0 6 12 18 24 30 36 42 48 54`

---

## 5. Design challenge and strawmen

### Benchmark: Zipf's law

> [!note] Zipf's law (natural language) If the most frequent word occurs about _t_ times, the 2nd occurs about _t/2_, the 3rd about _t/3_, the 4th about _t/4_, and so on.

- **Goal:** validate Zipf's law on real data
- **Method:** `python freq.py < data.txt | python sort.py`
- **Required:** an efficient symbol table

Moby Dick: "the" appears 13,967 times, "of" 6,415, "and" 6,247, matching the hypothesis roughly.

|File|Description|Words|Distinct|
|---|---|---|---|
|`mobydick.txt`|Melville's _Moby Dick_|210,028|16,834|
|`leipzig100k.txt`|100K random sentences|2,121,054|144,256|
|`leipzig200k.txt`|200K random sentences|4,238,435|215,515|
|`leipzig1m.txt`|1M random sentences|21,191,455|534,580|

Source: Wortschatz corpus, Universität Leipzig.

### Strawman I: ordered array

- Keep keys in order in one array, values in a parallel array
- Pro: uses fast sort (mergesort) and enables binary search
- **Fatal flaw:** inserting a new key means shifting larger entries (like insertion sort). Hypothesis: **quadratic** time on the benchmark.
- Also: how big should the arrays be?

### Strawman II: linked list

- Keep keys in order in a linked list, with a value in each node
- Pro: memory use meets spec
- **Fatal flaw:** binary search needs indexed access. You can't jump to the middle of a linked list, so you search sequentially. **Quadratic** on the benchmark.

### Required API and performance

|Requirement|Basic ST|
|---|---|
|`set`, `get`, `contains`|logarithmic (or better)|
|Memory|linear in collection size|
|Size limit|none in the code|

**Ordered symbol table** adds:

|Operation|Meaning|
|---|---|
|`min()`|smallest key|
|`max()`|largest key|
|`rank(key)`|number of keys less than `key`|
|`select(k)`|the _k_th smallest key (key of rank _k_)|
|`__iter__()`|all keys, sorted|

`min`, `max`, `rank`, `select` should also be logarithmic.

### Two implementation choices

||Hash table|BST|
|---|---|---|
|Keys must be|hashable|comparable|
|Ordered operations|not supported|supported|
|Issues|collisions (probing, separate chaining)|needs balance (AVL, red-black)|
|put/get/contains|amortized constant|guaranteed logarithmic (balanced)|

---

## 6. Hash tables

### Idea

- One array for keys, a parallel array for values, both of size **m**
- The key's **hash code** goes through a **hash function** to give an index in `[0, m)`

```python
def hashFunction(key, m):
    hashCode = hash(key)
    hashValue = hashCode % m
    return hashValue
```

### Hashable objects satisfy three properties

1. Can be compared for equality with other objects
2. Objects that compare equal have the **same hash code**
3. The hash code **never changes** during the object's lifetime

> [!tip] Consequence Mutable objects like `list` are not hashable, so they can't be `dict` keys. Tuples of hashables are fine.

### Implementation (separate chaining, `hashst.py`)

Each slot holds a small **list** of keys and a parallel list of values.

```python
class SymbolTable:
    def __init__(self, m=1024):
        self._m = m
        self._keys = stdarray.create2D(m, 0)
        self._vals = stdarray.create2D(m, 0)

    def __getitem__(self, key):
        i = hash(key) % self._m
        for j in range(len(self._keys[i])):
            if self._keys[i][j] == key:
                return self._vals[i][j]
        raise KeyError

    def __setitem__(self, key, val):
        i = hash(key) % self._m
        for j in range(len(self._keys[i])):
            if self._keys[i][j] == key:
                self._vals[i][j] = val
                return
        self._keys[i] += [key]
        self._vals[i] += [val]
```

> [!example] Lookup of key `TAA` with m = 5
> 
> 1. Hash value is 2
> 2. Look in the `_keys[2]` list
> 3. Linear search for `TAA` in that list, found at index 1
> 4. Return `_vals[2][1]`

### Performance

- Works well **if** the hash function distributes keys reasonably and chains stay short (about 1 to 8, effectively a constant)
- If chains grow, **resize the array** (space-time tradeoff)
- Result: `get`, `put`, `contains` are **amortized constant time** (the cost of resizing is spread over many operations)
- Does **not** support ordered operations

> [!warning] Slide inconsistency Slide 28 ticks "logarithmic" for the hash table, but the text on the same slide (and slide 24) says amortized **constant**. Constant is the better answer, and it meets the logarithmic spec.

---

## 7. Binary search trees

### Doubly-linked structures

With two links per node many structures are possible: doubly-linked list, circular list, binary tree, general trees. Maintenance can be complicated. This lecture uses the **binary tree**.

### Definition

> [!note] BST
> 
> - A **BST** is `None` or a reference to a **BST node** (the _root_)
> - A **BST node** holds a key, a value, and two BSTs (a _left_ and a _right_ subtree)
> - **Ordered:** every key in the left subtree is **smaller** than the node's key, every key in the right subtree is **larger**

```python
class _Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.left = None
        self.right = None
```

Example tree (from "it was the best of times"):

```
            it
           /  \
       best    was
               /
             the
            /   \
          of    times
```

### Standard operations

- Search for the value of a key
- Add a new key-value pair
- Traverse (visit every node in key order)
- Remove a key (not covered)

### Class skeleton

```python
class OrderedSymbolTable:
    def __init__(self):
        self._root = None
```

### Get (search)

Compare with the current node: go **left** if smaller, **right** if larger, return the value on a match (**search hit**), raise `KeyError` on reaching `None`.

```python
def _get(self, x, key):
    if x is None: raise KeyError
    if key < x.key:   return self._get(x.left, key)
    elif x.key < key: return self._get(x.right, key)
    else:             return x.val

def __getitem__(self, key):
    return self._get(self._root, key)
```

### Put (set)

Same search. On a match **update the value**. On reaching `None`, **return a new node**, which the caller links in.

```python
def _set(self, x, key, val):
    if x is None: return _Node(key, val)
    if key < x.key:   x.left  = self._set(x.left,  key, val)
    elif x.key < key: x.right = self._set(x.right, key, val)
    else:             x.val = val
    return x

def __setitem__(self, key, val):
    self._root = self._set(self._root, key, val)
```

> [!tip] Recursion pattern Each call returns the (possibly new) root of its subtree, and the caller reassigns `x.left` / `x.right` to it. That is how the new node gets attached without special-casing.

Examples: `put("the", 2)` is a hit that updates the value. `put("worst", 1)` goes right, right, then hits `None` and adds a new node.

### Contains

Same as `_get`, but returns `False` at `None` and `True` on a match.

```python
def _contains(self, x, key):
    if x is None: return False
    if key < x.key: return self._contains(x.left, key)
    if x.key < key: return self._contains(x.right, key)
    return True

def __contains__(self, key):
    return self._contains(self._root, key)
```

### Traversal: in-order gives sorted keys

1. Do the left subtree
2. Add the root's key to the list
3. Do the right subtree

```python
def _inorder(self, x, a):
    if x is None: return
    self._inorder(x.left, a)
    a += [x.key]
    self._inorder(x.right, a)

def __iter__(self):
    a = []
    self._inorder(self._root, a)
    return iter(a)
```

For the example tree the result is: `best it of the times was`.

### Trace of BST construction

Inserting `it was the best of times it was the worst`: `it` is the root, `was` goes right, `the` goes left of `was`, `best` goes left of `it`, `of` goes left of `the`, `times` goes right of `the`. Repeated keys just update values.

> [!warning] Slide typo The `isEmpty` method on slide 40 uses `self._root == null`. `null` is Java. In Python it should be `self._root is None`.

---

## 8. Analysis

### Cost depends on insertion order

|Case|Shape|Cost per operation|
|---|---|---|
|**Best**|perfectly balanced|logarithmic|
|**Typical / average** (random order)|roughly balanced|logarithmic|
|**Worst**|keys inserted in **sorted order**, tree becomes a linked list|linear (building the tree is quadratic)|

Sorted-order input happens in practice, so this is a real problem.

### Random-order model

> [!note] Proposition Building a BST by inserting _N_ randomly ordered keys into an empty tree uses about **2 N/ ln N** compares, which is about **1.39 N /lg N**.

The proof is an exercise in discrete maths, and it is worth revisiting in [[Discrete Math]] and an algorithms course. Random insertion gives a roughly balanced tree that tends to stay that way.

### Empirical test

Count words that appear more than once, doubling _N_ each time:

|N|T_N (s)|T_N / T_(N/2)|
|---|---|---|
|1 million|5||
|2 million|9|1.8|
|4 million|17|1.9|
|8 million|34|2.0|
|16 million|72|2.1|
|1 billion|4608|2|

A ratio slightly above 2 when doubling confirms **N log N** growth. It will scale, and it makes the 21M-word corpus easy, which brute force could not do.

### Balanced trees (performance guarantees)

**Problem:** keys may not arrive in random order, so a plain BST can degrade.

**Resolution:** balanced-tree algorithms apply simple transformations that **guarantee** balance.

- **AVL trees** (Adelson-Velskii and Landis, 1962): proved the concept
- **Red-black trees** (Guibas and Sedgewick, 1979): used in many modern systems

Red-black properties:

- Same number of **black** links on every path from root to leaf
- **No two red links in a row**
- Tree is nearly balanced and **guaranteed** to stay that way

> [!note] Proposition In a red-black tree of size _N_, `put()`, `get()`, `contains()` are **guaranteed** to use fewer than **2 lg N** compares.

Example: searching among 1 trillion customers takes fewer than 80 compares (lg 10^12 is about 40, so 2 lg N is about 80).

### Python and Java in practice

- **Python `dict`**: hash table with extra methods (e.g. delete), no ordered operations, preferred when order isn't needed

```python
st = {}
st['it'] = 2
st['the'] = 3
count1 = st['it']
```

- **Java**: `TreeMap` uses red-black trees, `HashMap` uses a hash table

---

## 9. Summary

|Structure|Notes|
|---|---|
|**BST**|Simple, usually efficient (logarithmic for random keys)|
|**Hash table**|More complicated, can be very efficient, **no ordered operations**|
|**Red-black tree**|BST variant, **guaranteed** efficient|

> [!success] Big idea We can implement associative arrays with just a log-factor extra cost, and many, many things are enabled by efficient symbol tables.

### Quick complexity cheat sheet

|Implementation|put / get / contains|Ordered ops (`min`, `rank`, ...)|
|---|---|---|
|Ordered array|get log N, **put linear**|yes|
|Linked list|**linear**|yes|
|Hash table|amortized O(1)|no|
|BST (random keys)|O(log N)|yes|
|BST (worst case)|O(N)|yes|
|Red-black tree|guaranteed O(log N)|yes|

### Self-test questions

- [ ] Why does mergesort + binary search fail for add/remove?
- [ ] What three properties make an object hashable?
- [ ] Why can't a `list` be a `dict` key?
- [ ] How does `_set` attach a new node to the tree?
- [ ] Why does in-order traversal give sorted keys?
- [ ] What input order makes a plain BST degrade to linear?
- [ ] When would you pick a hash table over a BST, and vice versa?