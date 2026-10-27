#CS144 
---


# 4.5 Small World Phenomenon

> [!summary] TL;DR A **graph** models pairwise connections (vertices + edges). We store it as a **dict of sets** (adjacency lists), find **shortest paths** with **breadth-first search (BFS)** using a FIFO queue, and test whether a graph is a **small world**: sparse, short average path length, high local clustering.

---

## 1. The Small World Phenomenon

- Idea: everyone is roughly **six handshakes** away from anyone else ("six degrees of separation").
- **Milgram's experiment (1960s):**
    - Participants got personal info (e.g. occupation, age) about a target person.
    - Goal: get a message to the target.
    - Rule: you may only forward it to someone you know **on a first-name basis**.
    - Result: delivered with an **average of ~5 intermediaries**.
- Graphs are the mathematical model for studying these pairwise connections; some graphs exhibit the small-world property.

### Applications

- **Sociology:** job hunting, marketing, spread of fame/fads, political representativeness, the **Kevin Bacon game**.
- **Other:** electronic circuits, neuron synchronisation, World Wide Web analysis, power grid design, protein interaction networks, spread of diseases & computer viruses, iterated Prisoner's Dilemma.

---

## 2. Graph Data Type

### Terminology

|Term|Meaning|
|---|---|
|**Graph**|A set of vertices + a set of edges|
|**Edge**|A connection between two vertices|
|**Neighbours**|Two vertices joined by an edge|
|**Degree**|Number of neighbours a vertex has|

### Example graph models

|Graph|Vertices|Edges|
|---|---|---|
|Communication|telephones, computers|fibre optic cables|
|Transportation|intersections, airports|highways, air routes|
|Software systems|functions|function calls|
|Internet|web pages|hyperlinks|
|Social|people, actors|friendships, movie casts|
|Neural networks|neurons|synapses|
|Scheduling|tasks|precedence constraints|
|Chemical|molecules|bonds|

### API

Graph-processing pattern: **build** the graph by adding edges, then **process** it by iterating over vertices and each vertex's neighbours.

**Constant-time operations**

|Operation|Description|
|---|---|
|`g.addEdge(v, w)`|add edge v–w|
|`g.countV()`|number of vertices|
|`g.countE()`|number of edges|
|`g.degree(v)`|number of neighbours of v|
|`g.hasVertex(v)`|is v a vertex?|
|`g.hasEdge(v, w)`|is v–w an edge?|

**Linear-time operations**

|Operation|Description|
|---|---|
|`Graph(file, delimiter)`|build from file (defaults: empty graph, whitespace delimiter)|
|`g.vertices()`|iterable over vertices|
|`g.adjacentTo(v)`|iterable over v's neighbours|
|`str(g)`|string representation|

> [!important] Space must be **linear in V + E**.

### Implementation: symbol table of sets

- Internal representation: `dict` where **key = vertex**, **value = set of neighbours**.
- Benefits:
    1. Efficient iteration over vertices.
    2. Efficient iteration over a vertex's neighbours.
    3. Space ∝ V + E.

```
H — A — G        vertex | neighbours
 \ / \ /           A    | B C G H
  B — C            B    | A C H
                   C    | A B G
                   G    | A C
                   H    | A B
```

```python
class Graph:
    def __init__(self, file=None, delimiter=None):
        self._e = 0          # number of edges
        self._adj = dict()   # vertex -> set of neighbours
        # (if file given: read each line, first token is v, add edge v–w for the rest)

    def addEdge(self, v, w):
        if not self.hasVertex(v): self._adj[v] = set()
        if not self.hasVertex(w): self._adj[w] = set()
        if not self.hasEdge(v, w):
            self._e += 1
            self._adj[v].add(w)
            self._adj[w].add(v)   # undirected: add both ways

    def adjacentTo(self, v): return iter(self._adj[v])
    def vertices(self):      return iter(self._adj)
    def hasVertex(self, v):  return v in self._adj
    def hasEdge(self, v, w): return w in self._adj[v]
    def countV(self):        return len(self._adj)
    def countE(self):        return self._e
    def degree(self, v):     return len(self._adj[v])

    def __str__(self):
        s = ''
        for v in self.vertices():
            s += v + '  '
            for w in self.adjacentTo(v):
                s += w + ' '
            s += '\n'
        return s
```

> [!tip] The output format of `str()` (one vertex per line, followed by its neighbours) doubles as a valid **input file format** for `__init__()`.

---

## 3. Graph Client Example — Inverting an Index (`invert.py`)

- **Movie–performer graph:** vertices are both movies _and_ performers; an edge connects a movie to each performer in it.
- File format (`movies.txt`): `Movie (Year)/Performer 1/Performer 2/...` with `/` as delimiter.
- Queries supported by the same graph:
    - Given a **performer** → list their movies.
    - Given a **movie** → list its performers.

```python
graph = Graph(file, delimiter)
while stdio.hasNextLine():
    v = stdio.readLine()
    if graph.hasVertex(v):
        for w in graph.adjacentTo(v):
            stdio.writeln('  ' + w)
```

### Kevin Bacon numbers

- **Game:** find the shortest chain of movies linking a performer to Kevin Bacon.
    - e.g. Kevin Kline → _French Kiss_ → Meg Ryan → _Sleepless in Seattle_ → Tom Hanks → _Apollo 13_ → Kevin Bacon.
- **How to compute:** shortest path in the performer–movie graph (→ BFS).
- The _Oracle of Bacon_ website does exactly this.

---

## 4. Shortest Paths in Graphs

> [!definition] Shortest path A path between two vertices with the **minimum number of edges** among all such paths. Its length is the **distance**.

### PathFinder API

|Operation|Description|
|---|---|
|`PathFinder(graph, s)`|compute all shortest paths from source `s`|
|`pf.distanceTo(v)`|distance from s to v|
|`pf.hasPathTo(v)`|is there a path from s to v?|
|`pf.pathTo(v)`|iterable over the path s → v|

Client (`separation.py`): build graph, create `PathFinder` from source, then for each queried vertex print the path and distance.

- e.g. `routes.txt` from JFK: JFK → ORD → PHX → LAX (distance 3); JFK → ORD → DFW (distance 2).

### Idea: distances in layers

- Source is at distance **0**.
- Its neighbours are at distance **1**.
- _Their_ unvisited neighbours are at distance **2**, and so on.

### Breadth-First Search (BFS)

**Goal:** from source `s`, find the shortest path to every other vertex.

1. Put `s` on a **FIFO queue**.
2. Repeat until the queue is empty:
    - Dequeue the least recently added vertex `v`.
    - Add each of `v`'s **unvisited** neighbours to the queue and mark them visited.

> [!important] Key observation Vertices are visited in **increasing order of distance** from `s` _because_ the queue is FIFO.

```python
from linkedqueue import Queue

class PathFinder:
    def __init__(self, graph, s):
        self._distTo = dict()   # distance from s
        self._edgeTo = dict()   # previous vertex on shortest path from s
        queue = Queue()
        queue.enqueue(s)
        self._distTo[s] = 0
        self._edgeTo[s] = None
        while not queue.isEmpty():
            v = queue.dequeue()
            for w in graph.adjacentTo(v):
                if w not in self._distTo:          # unvisited
                    queue.enqueue(w)
                    self._distTo[w] = 1 + self._distTo[v]
                    self._edgeTo[w] = v

    def distanceTo(self, v): return self._distTo[v]
    def hasPathTo(self, v):  return v in self._distTo

    def pathTo(self, v):
        path = []
        while v is not None:       # walk back to s via _edgeTo
            path += [v]
            v = self._edgeTo[v]
        return reversed(path)
```

- `_distTo` doubles as the **visited** set.
- `_edgeTo` stores a **parent pointer** for each vertex, forming a shortest-paths tree; `pathTo` follows it backwards then reverses.

### BFS applications

Particle tracking, image processing, web crawling, routing internet packets. Extension: map route-finding (e.g. Google Maps, with weighted edges).

---

## 5. Small-World Graphs

A graph is a **small world** if it is:

1. **Sparse** — average degree is small relative to V.
2. **Short average path length** — few hops between typical pairs.
3. **Locally clustered** — your neighbours tend to be neighbours of each other.

### Measures

**Average degree** $$\bar d = \frac{2E}{V}$$

**Average path length** (over all ordered pairs of distinct vertices) $$L = \frac{\sum_{v}\sum_{w} \text{dist}(v,w)}{V(V-1)}$$

**Clustering coefficient** — for vertex $v$ with degree $d_v$: $$C_v = \frac{\text{actual edges among } v\text{'s neighbours}}{\binom{d_v}{2}}, \qquad C = \frac{1}{V}\sum_v C_v$$

### Worked example (tinygraph: A, B, C, G, H; 7 edges)

|Vertex|Degree|Possible neighbour edges|Actual|
|---|---|---|---|
|A|4|6|3|
|B|3|3|2|
|C|3|3|2|
|G|2|1|1|
|H|2|1|1|

- Average degree = 14 / 5 = **2.8**
- Average path length = 13 / 10 pairs = **1.3** (only B–G, C–H, G–H have length 2)
- Clustering coefficient = (3/6 + 2/3 + 2/3 + 1 + 1) / 5 ≈ **0.767**

### Why the standard models fail

|Model|Problem|
|---|---|
|**Complete graph**|not sparse — too many edges|
|**Ring graph**|clustered, but too many long paths|
|**Random graph**|short paths, but **not locally clustered**|

### `smallworld.py`

```python
def averageDegree(graph):
    return 2.0 * graph.countE() / graph.countV()

def averagePathLength(graph):
    total = 0
    for v in graph.vertices():
        pf = PathFinder(graph, v)          # one BFS per vertex
        for w in graph.vertices():
            total += pf.distanceTo(w)
    return 1.0 * total / (graph.countV() * (graph.countV() - 1))

def clusteringCoefficient(graph):
    total = 0
    for v in graph.vertices():
        possible = graph.degree(v) * (graph.degree(v) - 1)
        actual = 0
        for u in graph.adjacentTo(v):
            for w in graph.adjacentTo(v):
                if graph.hasEdge(u, w):
                    actual += 1
        if possible > 0:
            total += 1.0 * actual / possible
    return total / graph.countV()
```

> [!note] Ordered pairs `clusteringCoefficient` loops over **ordered** pairs (u, w), so both `actual` and `possible` = $d(d-1)$ are double the unordered counts — the ratio is the same as $\text{actual}/\binom{d}{2}$.
> 
> `averagePathLength` assumes the graph is **connected** (`distanceTo` fails for unreachable vertices).

---

## Key Takeaways

- Graph = vertices + edges; represent as **dict of sets** for O(V + E) space and fast neighbour iteration.
- **BFS + FIFO queue** ⇒ shortest paths (by edge count) in unweighted graphs.
- `_distTo` tracks distance/visited; `_edgeTo` reconstructs paths.
- Small-world = **sparse + short paths + clustered**.