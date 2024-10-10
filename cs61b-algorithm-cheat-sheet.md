---
description: Robert Lu
---

# CS61B Algorithm Cheat Sheet

Asymptotics Runtime

**Runtime Measurement.**&#x20;

```java
// record in ms
long start = System.currentTimeMillis();
doSomeThing();
long end = System.currentTimeMillis();

// record in ns
long start_ns = System.nanoTime();
doSomeThing();
long end_ns = System.nanoTime();

// use stopwatch
Stopwatch sw = new Stopwatch();
doSomeThing();
return sw.elapsedTime();
```

**Algorithm Scaling** how the runtime of a piece of code grows as a function of its input size.

* Θ : there exists positive constants k1 and k2 such that：<img src="https://lh7-rt.googleusercontent.com/slidesz/AGV_vUej-3AprJ2y_kgDv8erZYbl8XYEsj4eoxt6A5d9fTaqH9g5FYsjBmFBwA2W8gjxEczZo6eIcsBjNj6-4rTTGgWzob40N5KiFpOAOCWcy6a0WVAmWlD0apXS7EMcGkY533Wmh4PnD15StQ_9ztxgVW-xvq9Wpak3=s2048?key=IfusJRdu1zosN8O2yRFplg" alt="" data-size="line">, [Demo](https://sp18.datastructur.es/materials/demos/asymptotics.html?rN=4\*N%5E2+40\*sin\(N\)\&fN=N%5E2\&k1=3\&k2=5\&maxN=15\&maxY=1000).

<figure><img src=".gitbook/assets/image (108).png" alt="" width="375"><figcaption></figcaption></figure>

* O : there exists positive constants k2 such that：<img src="https://lh7-rt.googleusercontent.com/slidesz/AGV_vUcBLSKtswaEPE7bNHL4So6NCQicDq6skCJsO_xGkTqVFrZEgWt7EdbhNTfpTZJprL_TU2Hg0f7i0T0OjWNKxQHU1iI_VfYkTaiUQSCb0etHQUSyPULMYP1iAYtPEYM4177vhZr5Wj0dzCdH2tAFLluWvZn7B3Q=s2048?key=IfusJRdu1zosN8O2yRFplg" alt="" data-size="line">, [Demo](https://sp19.datastructur.es/materials/demos/asymptotics.html?rN=4\*N%5E2+40\*sin\(N\)\&fN=N%5E4\&k1=0\&k2=5\&maxN=15\&maxY=1000).

<figure><img src=".gitbook/assets/image (109).png" alt="" width="375"><figcaption></figcaption></figure>

* Important sums:&#x20;
  * Sum of first 𝑛 natural numbers: 1 + 2 + 3 + … + n = 𝑛 ( 𝑛 + 1 ) / 2 ⟹ Θ ( 𝑛^2 )
  * Sum of first 𝑛 powers of 2: 1 + 2 + 4 + 8 + … + Q = 2Q−1 ⟹ Θ(Q) (if Q = 2^n, Θ(2^n), if Q = n, Θ(n)).

## Sorting and Search: Binary search and Mergesort

* Binary Search
  * Repeatedly divide the search interval in half until the value is found or the interval is empty

```java
// 
Let m = middle element
Let v = value (searching)
if m == v:
    return true;
elif m > v:
    search lower half
elif m < v:
    search upper half 

```

* Mergesort
  * Break down array into separate arrays with one element each then merge them back together to form a sorted array&#x20;

<figure><img src=".gitbook/assets/image (24).png" alt="" width="563"><figcaption></figcaption></figure>

## Disjoint Sets

* Operations
  * `connect(int p, int q)` also called `union()`
  * `isConnected(int p, int q)` also called `connected()`
  * `find(int p)` finds the root of the set p belongs to
* Goal:&#x20;
  * Support large number of elements 𝑁 and method calls 𝑀
* Structure:
  * ListOfSetsDS: Store connected components as a List of Sets (slow, complicated).
  * QuickFindDS: Store connected components as set ids. _Indicies_ represent ID, _values_ represent the parent.
  * QuickUnionDS: Store connected components as parent ids.
    * Parent = − 1 if the element has no parent&#x20;
  * WeightedQuickUnionDS: Track the size of each set, and use size to decide on new tree root. Connect the smaller branch to the larger branch.
    * Parent = − 𝑘 where k is the number of nodes (including itself)&#x20;
    * When calling `connect(p, q)`, modify the parent node sizes appropriately&#x20;
    * To break ties, connect p to q&#x20;
  * WeightedQuickUnionWithPathCompressionDS: On calls to `connect` and `isConnected`, set parent id to the root for all items seen, results in **nearly flat trees.**

<figure><img src=".gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

## Types of collections

* List - LinkedList, ArrayList (Ordered & Allow duplicates)
* Set - HashSet, TreeSet (Unordered & No duplicates)
* Map - HashMap, TreeMap  (Key-value pairs & No duplicate key)
* Abstract - Weighted QU, Priority Queue, Trie, Kd-tree&#x20;

Map只是将Set中的Element换成了Key-Value Pair

<figure><img src=".gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

```java
// Using ADT
List<String> al = new ArrayList<>();
List<String> ll = new LinkedList<>();

Set<String> hs = new HashSet<>();
Set<String> ts = new TreeSet<>();
Set<String> lhs = new LinkedHashSet<>();

Map<String, Integer> hm = new HashMap<>();
Map<String, Integer> tm = new TreeMap<>();

Queue<String> q = new ArrayDeque<>();
Deque<String> dq = new ArrayDeque<>();
Queue<String> pq = new PriorityQueue<>();

Stack<String> s = new Stack<>();

WeightedQuickUnionUF wqu = new WeightedQuickUnionUF(100);
```

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>



An Abstract Data Type (ADT) is defined only by its operations, not by its implementation.&#x20;

**ADT and Operations**

**The Disjoint Sets**

**ADT**

**Operations**

* connect(x, y): Connects x and y.
* isConnected(x, y): Returns true if x and y are connected. Connections can be transitive, i.e. they don’t need to be direct.

### Runtimes (overall)&#x20;

<figure><img src=".gitbook/assets/image (26).png" alt="" width="563"><figcaption></figcaption></figure>

* Average BSTs are logN but worst case are 𝑁 .
* We assume hash tables have evenly spread objects
* Heap is Θ ( log 𝑁) time AMORTIZED (not including resizing)
  * Resize = Θ ( 𝑁 )

## Trees

### BST

* Every key in the left subtree is less than X's key, right is greater.&#x20;
* Worst case height/contains/add is Θ ( 𝑁 ) , best case Θ ( log  𝑁 )&#x20;
* Operations
  * insert() go left if less than X's key, right if greater
  * delete() promote the rightmost element of the left branch or the leftmost element of the right branch to root.&#x20;

### B-Tree (2-3 Tree & 2-3-4 Tree)&#x20;

* Add new nodes horizontally in a leaf&#x20;
  * Once a node exceeds an arbitrary limit (2 or 3), send a middle element up to parent node&#x20;
  * Height only increases once root node splits. (This is why B-Tree is **Balanced by construction**)
* Invariants:&#x20;
  * All leaves must be the same distance from the source.&#x20;
  * A non-leaf node with k items must have exactly k+1 children.&#x20;

### Left Leaning Red-Black Tree (LLRB)&#x20;

* Represent a 2-3 Tree as a BST using "red links" as glu e (_<mark style="color:blue;">red</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;">links to indicate two nodes that would be in the same 2-3 Node)</mark>
  * Balanced by construction&#x20;
* Invariants:&#x20;
  * Red links lean left.
  * <mark style="color:blue;">No node ever has 2 red links (It wouldn’t be a valid node in a 2-3 Tree if it did)</mark>
  * <mark style="color:blue;">Every path from the root to a leaf has the same number of</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">black links</mark>_. This is because every leaf in a 2-3 tree has same numbers of links from root. Therefore, the tree is balanced.
  * Operations are 𝑂(log𝑁)&#x20;
*   Operations:&#x20;

    * Insert: Always use red link when inserting, insert as usual in BST, then rotate accordingly.

    <figure><img src=".gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

    *   Always insert with a red link at the correct location. Then use the following three operations to “fix” or LLRB tree.&#x20;

        * If there is a right leaning red link, rotate that node left.

        <figure><img src=".gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

        * If there are two consecutive left leaning links, rotate right on the top node.

        <figure><img src=".gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

        * If there is a node with two red links to children, flip all links with that node.

        <figure><img src=".gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>
    * **Delete** 我们不能删除黑色的节点，因为这样会破坏黑高。所以我们需要保证我们最后删除的节点是红色的。



    <figure><img src=".gitbook/assets/image (20).png" alt="" width="375"><figcaption></figcaption></figure>

## Hashing&#x20;

* Integers overflow at 2 , 147 , 483 , 647 which can cause hash collisions (inevitable)&#x20;
* Store items in buckets (an ArrayList or LinkedList)&#x20;
  * If bucket h is empty, create a new list with element x&#x20;
  * Otherwise add it to the list at bucket h&#x20;

### Hashtable&#x20;

* Use modulo operator % to reduce bucket count to create a HashTable&#x20;
  * Use Math.floorMod() to avoid negative indices&#x20;
  * Or (key.hashCode() & 0x7FFFFFFF) % buckets;&#x20;
* Increase the number of buckets M, N items, such that 𝑁/𝑀 > 1.5 , then double M
  * 𝑁/𝑀 is the load factor
* Worst case for all operations isΘ ( 𝑁/𝑀 ) = Θ ( 1 ) on average, assuming evenly spread items&#x20;
  * Resizing takes Θ ( 𝑁 ) time for some add operations&#x20;
* All objects implement a .hashCode() method&#x20;
* <mark style="color:red;">Warnings</mark>&#x20;
  * <mark style="color:red;">Never store keys that change or their hashCode will change and get lost</mark>&#x20;
  * <mark style="color:red;">Never override .equals() without overriding .hashCode()</mark>&#x20;

## Heaps and PQs&#x20;

#### Priority Queues:&#x20;

* Allow tracking and removal of the smallest item

```java
// Some code
public interface MinPQ {
    //Adds the item to the priority queue. 
    public void add(Item x); 
    //Returns smallest item in the priority queue
    public Item getSmallest(); 
    //Removes smallest item from the priority queue 
    public Item removeSmallest(); 
    //Returns the size of the priority queue. 
    public int size();
}

```

#### Heaps:&#x20;

* Represented by BSTs&#x20;
  * Keys are stored in an array with zeroth element blank, first being the root of the tree with the second and third element being its children and so on
* Binary tree must be complete and obey the min-heap property&#x20;
  * Min-heap: Every node is less than or equal to both of its children&#x20;
  * Complete: Missing items only at the bottom level (if any), all nodes are as far left as possible.&#x20;
    * leftChildIndex = 2k&#x20;
    * rightchildIndex = 2k+1&#x20;
* Operations:&#x20;
  * add(x): Add to end of heap temporarily then swim up hierarchy to rightful place&#x20;
  * removeSmallest(): Remove the root, put the last item in the heap into the root, then sink down the hierarchy to its rightful place&#x20;
* Heap Implementation of Priority Queue&#x20;

## Algorithm Summery

* <mark style="color:red;">The sort problem</mark> is to take a sequence of objects and put them into the correct order.&#x20;
* <mark style="color:red;">The search problem</mark> is to store a collection of objects such that they can be rapidly retrieved (i.e. how do we implement a Map or Set).&#x20;
* BST maps are roughly analagous to <mark style="color:red;">comparison based sorting</mark>
* hash maps are roughly analagous to <mark style="color:red;">counting based (a.k.a. integer) sorting</mark>.
* Tries are roughly analogous to LSD or MSD sort (sorting by digits).

## Tries&#x20;

<figure><img src=".gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

* Basic idea: Store each letter of the string as a node in a tree.&#x20;
* Mark the end of each key/string&#x20;
* Trie Terminology:
  * Length of string key usually represented by L.
  * Alphabet size usually represented by R.
  * R-way Trie.
* Trie Implementations:&#x20;
  * DataIndexedCharMap: Very fast, but memory hungry.
    * Search runtime: Θ ( 1 )&#x20;
  * Hash Table: Almost as fast, uses less memory.&#x20;
    * Search runtime: 𝑂 ( 𝑅 ) where R is size of alphabet&#x20;
  * Balanced BST: A little slower than Hash Table and uses a similar amount of memory as Hash Table&#x20;
    * Search runtime: 𝑂 ( log  𝑅 ) where R is size of alphabet&#x20;
  * Don't have to worry very much about runtime because alphabet size is fixed: can think of all three as Θ ( 1 )&#x20;
* Trie Operations:&#x20;
  * collect(): Collects all keys in a trie&#x20;
  * keysWithPrefix(str): Collects all keys starting with prefix str&#x20;
  * longestPrefixOf(key): Returns the longest prefix of a key in the Trie&#x20;

## TernarySearchTree：

* Tree中有多少个深色节点，就有多少个Node；
* search：如果是往中间走，就要减少一个char往下；如果是往两边就不能减少，并且继续搜索；搜索"boat"，在"o"往下接下来搜索的是"at"，相当于把o吃掉了；搜索"bat"，在"o"往左，"a"没有被吃掉，因为"a" < "o"，往左边走。
* boat, boats, bat, bats
* <mark style="color:red;">TST和传统的Trie相比有什么优点？</mark>

<figure><img src=".gitbook/assets/image (125).png" alt="" width="252"><figcaption></figcaption></figure>



#### Searching string in the TST:

## K-d Trees&#x20;

* Root nodes partition by left-right (by x)&#x20;
* Alternates axis partitioning at every depth level&#x20;
* Explore the better side first&#x20;
* If the worse side has a possible point closer than current best, explore that&#x20;
  * Compare the appropriate axis value(x if partition by x) to see if that distance is < best&#x20;
* Tie break: equal in 1 dimension put as right child&#x20;

## Graphs&#x20;

Graph API:

```java
//Create empty graph with v vertices 
public Graph(int V): 
//add an edge v-w 
public void addEdge(int v, int w): 
//vertices adjacent to v 
Iterable adj(int v): 
int V(): //number of vertices 
int E(): //number of edges 
```

#### Traversals&#x20;

* Preorder/DFS: Root, left, right&#x20;
* Inorder: Left, root, right&#x20;
* Postorder/DFS: Left, right, root&#x20;
* Level order/BFS: Top to bottom, left to right&#x20;

#### DFS&#x20;

* Find a path from s to every reachable vertex.&#x20;
* Recursively search children until you reach the end and return T/F&#x20;
* Constructor: 𝑂 (𝑉+𝐸) time, Θ (𝑉) space

```java
private boolean[] marked; 
private int[] edgeTo;
private void dfs(Graph G, int v) { 
    marked[v] = true; 
    for (int w : G.adj(v)) { 
        if (!marked[w]) { 
            edgeTo[w] = v; 
            dfs(G, w); 
        }
    }
} 
```

<figure><img src=".gitbook/assets/image (131).png" alt="" width="375"><figcaption><p>Use function calling stack to analyse graph DFS</p></figcaption></figure>

**DFS with Stack**

* <mark style="color:red;">Should mark a vertex after it has been visited;（在访问之后才能标记，否则顺序DFS顺序会出错：例如在假如fringe就进行标记）</mark>
* <mark style="color:red;">Should visit a vertex if a popped vertex is not marked;  （如果在popped之后直接visit，可能出现被访问两次的情况）</mark>

```
// Some code
create the fringe, which is an empty Stack
push the start vertex onto the fringe and mark it
while the fringe is not empty:
    pop a vertex off the fringe
    if the vertex not marked:
        visit and mark the vertex you just popped
        for each neighbor of the vertex:
            if neighbor not marked:
                push neighbor onto the fringe
```

#### BFS&#x20;

* Find a shortest path from s to every reachable vertex.&#x20;
* Initialize a queue(fringe) with a starting vertex s and mark that vertex.&#x20;
* Use an adjacency list (array of lists, index = vertex number)&#x20;
  * Distance to all items on queue is always 𝑘 or 𝑘 + 1 for some 𝑘&#x20;
* Runtime&#x20;
  * Constructor: 𝑂 (𝑉+𝐸) time, Θ (𝑉) space&#x20;
  * Printing all connected vertices: Θ (𝑉+𝐸)&#x20;
  * Space used: Θ ( 𝑉+𝐸)

```java
private boolean[] marked; 
//marked[v] is true iff v connected to s 
private int[] edgeTo; 
//edgeTo[v] is previous vertex on path from s to v

private void bfs(Graph G, int s) { 
    Queue fringe = new Queue(); 
    fringe.enqueue(s); 
    marked[s] = true; 
    
    while (!fringe.isEmpty()) { 
        int v = fringe.dequeue(); 
        for (int w : G.adj(v)) { 
            if (!marked[w]) { 
                fringe.enqueue(w); 
                marked[w] = true; 
                edgeTo[w] = v; 
                distTo[w] = distTo[v] + 1; //if keeping track of distance 
            }
        }
    }
} 
```

#### Shortest Paths&#x20;

* BFS vs DFS: Both accurate, same time efficiency, BFS guarantees shortest paths&#x20;
  * DFS is worse for spindly graphs, BFS worse for bushy graphs&#x20;
  * BFS does not use edge weights&#x20;
* Terminology&#x20;
  * All shortest paths from a source vertex s will be a tree
  * Edge relaxation = use the edge only if it yields a better distance, add that to the SPT

#### Dijkstra's Algorithm&#x20;

Dijkstra’s algorithm is a method of finding <mark style="color:red;">the shortest path from one node to every other node in the graph</mark>. You use a <mark style="color:red;">priority queue</mark> that sorts vertices based off of their distance to the root node. Guaranteed to give best result assuming <mark style="color:red;">non-negative edges.</mark>

**Steps**:

1. Pop node from the top of the queue - this is the current node.
2. Add/update distances of all of the children of the current node in the PQ. (So-called edge-relaxing)
3. Re-sort the priority queue (technically the PQ does this itself).
4. Finalize the distance to the current node from the root.
5. Repeat while the PQ is not empty.

<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption><p>Example perform dijkstra from A</p></figcaption></figure>

**EdgeTo is a tree.**

{% code lineNumbers="true" %}
```java
//Dijkstra’s Pseudocode
PQ = new PriorityQueue()
PQ.add(A, 0)
PQ.add(v, infinity) # (all nodes except A).

distTo = {} # map
distTo[A] = 0
distTo[v] = infinity # (all nodes except A).

while (not PQ.isEmpty()):
    popNode, popPriority = PQ.pop()

    for child in popNode.children:
        if PQ.contains(child):
            potentialDist = distTo[popNode] +
            edgeWeight(popNode, child)
            if potentialDist < distTo[child]:
                distTo.put(child, potentialDist)
                PQ.changePriority(child, potentialDist)
```
{% endcode %}

* Invariants and properties
  * edgeTo\[v] is the best known predecessor of v.
  * distTo\[v] is the best known total distance from source to v.
  * PQ contains all unvisited vertices in order of distTo.
  * Always visits vertices in order of total distance from source.&#x20;
  * Relaxation always fails on edges to visited (white) vertices.
* Runtime
  * PQ add: 𝑂 ( 𝑉log𝑉)
  * PQ removeSmallest: 𝑂(𝑉log𝑉)
  * PQ changePriority: 𝑂(𝐸log𝑉 )
  * Overall: 𝑂 (𝑉log𝑉) + 𝑂(𝑉log𝑉) + 𝑂(𝐸log𝑉)

#### A\* Algorithm

* Use a heuristic to guide algorithm in the right direction
* Visit vertices in order of d(source, v) + h(v, goal)&#x20;
  * h is a heuristic (does not change while algorithm runs)
  * Constant heuristics means it just does Dijkstra's
  * Want h to be admissible(cannot overestimate the actual shortest path) and consistent: ℎ \[ 𝑣 ] ≤  weight ( 𝑣 , 𝑤 ) + ℎ \[ 𝑤 ] for each neighbor of w
* Not every vertex is visited
* Result is not a shortest paths tree, only shortest path to goal

A\* is a method of finding <mark style="color:red;">the shortest path from one node to a specific other node in the graph</mark>. It operates similarly to Dijkstra’s except for that we use a (given) heuristic to estimate a vertex’s distance from the goal.

Steps:

1. Pop node from the top of the queue - this is the current node.
2. Add/update distances of all of the children of the current node. This distance will be the sum of the <mark style="color:red;">distance up to that child node</mark> and <mark style="color:red;">our guess of how far away the goal node is (our heuristic)</mark>. (**disTo数组还是按照真实的来记录，但是在PQ中要加上heuristic**)
3. Re-sort the priority queue.
4. Check if we’ve hit the goal node (if so we stop).
5. Repeat while the PQ is not empty.

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

{% code lineNumbers="true" %}
```java
// A* Pseudocode
PQ = new PriorityQueue()
PQ.add(A, h(A))
PQ.add(v, infinity) # (all nodes except A).

distTo = {} # map
distTo[A] = 0
distTo[v] = infinity # (all nodes except A).

while (not PQ.isEmpty()):
    poppedNode, poppedPriority = PQ.pop()
    if (poppedNode == goal): terminate

    for child in poppedNode.children:
        if PQ.contains(child):
            potentialDist = distTo[poppedNode] +
            edgeWeight(poppedNode, child)
            
            if potentialDist < distTo[child]:
                distTo.put(child, potentialDist)
                PQ.changePriority(child, potentialDist + h(child))
```
{% endcode %}

#### MST

* A MST T is a subgraph of G.
* Connected, acyclic, includes all vertices
* <mark style="color:red;">Spanning tree whose edge weights have the minimum sum</mark>
* Similar to shortest path trees (SPT) but not always the same
  * SPT depends on source vertex while MST does not
  * SPT：从source到其他节点的最短路径，一般是Directed Graph；
  * MST：从任意节点开始，连接所有节点的重量最小的Tree（如果所有节点不是connected会怎么样？那么生成的结果就和选取的节点有关了，只有和选中节点connected的节点才能出现在结果中），一般是Undirected Graph；
* Edge weights not unique ⟹ multiple different MSTs
* Cut property:&#x20;
  * Divide vertices into 2 sets arbitrarily.&#x20;
  * The minimum crossing edge for ANY cut is in the MST&#x20;
* Prim's Algorithm&#x20;
  * Choose an arbitrary source node&#x20;
  * Repeatedly add the shortest edge to connect a new vertex&#x20;
  * Very similar to Dijkstra's except consider distance from entire tree (any vertex in tree) instead of from source&#x20;
  * Runtime: 𝑂 (𝐸log𝑉 ) (Assuming 𝐸 >> 𝑉 )&#x20;

<pre data-overflow="wrap"><code>// Prim’s Algorithm

Start with any node.
Add that node to the set of nodes in the MST.
While there are still nodes not in the MST:
    Add <a data-footnote-ref href="#user-content-fn-1">the lightest edge</a> from a node in the MST that leads to a new node that is unvisited.
    Add the new node to the set of nodes in the MST.
</code></pre>

* Krusgal's Algorithm
  * Add the lowest weight edge to the tree as long as it doesn't create a cycle&#x20;
  * Runtime: 𝑂 ( 𝐸  log  𝐸 ) , if pre-sorted list of edges: 𝑂 ( 𝐸  log\*  𝑉 ) (using a WQUPC)

```
// Kruskal’s Algorithm
While there are still nodes not in the MST:
    Add the lightest edge that doesn’t create a cycle.
    Add the endpoints of that edge to the set of nodes in the MST.
```

Trie and Ternary Search Tree

**Topological Sorting**

Topological Sort is a way of transforming a directed acyclic graph into a linear ordering of vertices, where for every directed edge u v, vertex u comes before v in the ordering.

<figure><img src=".gitbook/assets/image (13).png" alt="" width="256"><figcaption></figcaption></figure>

Perform a DFS traversal <mark style="color:red;">start from every vertex with indegree 0</mark>, <mark style="color:red;">NOT clearing markings in between traversals</mark>.

<mark style="color:red;">注意是先完成所有zero in-degree的节点为起点的DFS后，得到了包含左右节点的PostOrder顺序，最后再做reverse</mark>；

* Record DFS postorder in a list: \[7, 4, 1, 3, 0, 6, 5, 2]
* Topological ordering is given by the reverse of that list (reverse postorder): \[2, 5, 6, 0, 3, 1, 4, 7]

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

**DAG Shortest Paths**

<mark style="color:red;">**待确认：Topological Sorting的前提必须是DAG，但是DFS、BSF、Dijkstra's、Prim's和Kruskal's对于cyclic graph也是有效的。**</mark>

在DAG中找到最短路径，路径可以为负。

1，执行Topological sorting；

2，执行Dijkstra's，按照Topological sorting的结果进行Relaxing。

<mark style="color:red;">Topological Sorting保证当前节点进行Relaxing的时候，只有之前的节点指过来，不会有之后的节点只过来。</mark>

**The Longest Paths**

* Best known algorithm is exponential (extremely bad).
* Perhaps the most important unsolved problem in mathematics.



Graph Problems

|                 |                  |
| --------------- | ---------------- |
| Graph Algorithm | Runtime          |
| DFS             | O (V + E)        |
| BFS             | O (V + E)        |
| Dijkstra's      | O((V + E) log V) |
| A\*             | O((V + E) log V) |
| Prim’s          | O((V + E) log V) |
| Kruskal’s       | O(E log E)       |

&#x20;&#x20;

<table data-header-hidden><thead><tr><th width="181"></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Problem</strong></td><td><strong>Problem Description</strong></td><td><strong>Solution</strong></td><td><strong>Efficiency</strong></td></tr><tr><td>paths</td><td>Find a path from s to every reachable vertex.</td><td><p>DepthFirstPaths.java</p><p><a href="https://docs.google.com/presentation/d/1lTo8LZUGi3XQ1VlOmBUF9KkJTW_JWsw_DOPq8VBiI3A/edit?usp=sharing">Demo</a></p></td><td><p>O(V+E) time</p><p>Θ(V) space</p></td></tr><tr><td>shortest paths</td><td>Find the shortest path from s to every reachable vertex.</td><td><p>BreadthFirstPaths.java</p><p><a href="https://docs.google.com/presentation/d/1JoYCelH4YE6IkSMq_LfTJMzJ00WxDj7rEa49gYmAtc4/edit?usp=sharing">Demo</a></p></td><td><p>O(V+E) time</p><p>Θ(V) space</p></td></tr><tr><td>shortest weighted paths</td><td>Find the shortest path, considering weights, from s to every reachable vertex.</td><td><p>DijkstrasSP.java</p><p><a href="https://docs.google.com/presentation/d/1_bw2z1ggUkquPdhl7gwdVBoTaoJmaZdpkV6MoAgxlJc/pub?start=false&#x26;loop=false&#x26;delayms=3000">Demo</a></p></td><td><p>O(E log V) time</p><p>Θ(V) space</p></td></tr><tr><td>shortest weighted path</td><td>Find the shortest path, consider weights, from s to some target vertex</td><td><p>A*: Same as Dijkstra’s but with h(v, goal) added to priority of each vertex.</p><p><a href="https://docs.google.com/presentation/d/177bRUTdCa60fjExdr9eO04NHm0MRfPtCzvEup1iMccM/edit#slide=id.g369665031c_0_350">Demo</a></p></td><td><p>Time depends on heuristic.</p><p>Θ(V) space</p></td></tr></tbody></table>

&#x20;

| minimum spanning tree | Find a minimum spanning tree. | <p>LazyPrimMST.java</p><p><a href="https://docs.google.com/presentation/d/18leOHESniaJqqehiTR-YAL4WeEEcHJyRB9aw_S1FLG0/edit#slide=id.g772f8a8e2_0_28">Demo</a></p> | <p>O(???) time</p><p>Θ(???) space</p>   |
| --------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------- |
| minimum spanning tree | Find a minimum spanning tree. | <p>PrimMST.java</p><p><a href="https://docs.google.com/presentation/d/1GPizbySYMsUhnXSXKvbqV4UhPCvrt750MiqPPgU-eCY/edit#slide=id.g9a60b2f52_0_0">Demo</a></p>      | <p>O(E log V) time</p><p>Θ(V) space</p> |
| minimum spanning tree | Find a minimum spanning tree. | <p>KruskalMST.java</p><p><a href="https://docs.google.com/presentation/d/18leOHESniaJqqehiTR-YAL4WeEEcHJyRB9aw_S1FLG0/edit#slide=id.g5347e2c8f_2213">Demo</a></p>  | <p>O(E log E) time</p><p>Θ(E) space</p> |

| **Problem**        | **Problem Description**                                      | **Solution**                                                                                                                                                                                                                             | **Efficiency**                        |
| ------------------ | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| topological sort   | Find an ordering of vertices that respects edges of our DAG. | <p><a href="https://docs.google.com/presentation/d/1hFy9WhfnYqgjedUTBWchOljtzNbZ3DFTX48mq0PdDQk/edit#slide=id.g99668982c_1_693">Demo</a></p><p>Code: Topological.java</p>                                                                | <p>O(V+E) time</p><p>Θ(V) space</p>   |
| DAG shortest paths | Find a shortest paths tree on a DAG.                         | <p><a href="https://docs.google.com/presentation/d/1CfnLS3FSXV8X2sXPTravZGXeBUUkcFQv7Uf2iGWGUfs/edit#slide=id.g76e0dad85_2_380">Demo</a></p><p>Code: <a href="https://algs4.cs.princeton.edu/44sp/AcyclicSP.java">AcyclicSP.java</a></p> | <p>O(V+E) time</p><p>Θ(V) space</p>   |
| longest paths      | Find a longest paths tree on a graph.                        | No known efficient solution.                                                                                                                                                                                                             | <p>O(???) time</p><p>O(???) space</p> |
| DAG longest paths  | Find a longest paths tree on a DAG.                          | <mark style="color:red;">Flip signs, run DAG SPT, flip signs again.</mark>                                                                                                                                                               | <p>O(V+E) time</p><p>Θ(V) space</p>   |

&#x20;

关于Graph的Asymptotics

1. DFS和BFS：遍历一遍全图，要走一遍左右的Vertex和Edge，所以复杂度是O(V+E)；
2. Dijkstra's、A\*和Prim's：在遍历的基础上，每次从一个PQ获取最小的距离，复杂度为O((V+E)_log_V)；
3. Kruskal's：遍历每条边，每次从一个PQ中获取最小距离，复杂度为O(E_log_E)；

[^1]: 
