---
description: 'Author: Josh Hug and Kartik Kapur'
---

# 25. Minimum Spanning Trees, Kruskal's, Prim's

### QA&#x20;

Linked [here](https://youtu.be/nLoDF76mlm4).

### Check-in Exercise [#](https://fa22.datastructur.es/materials/lectures/lec26/#check-in-exercise) <a href="#check-in-exercise" id="check-in-exercise"></a>

Linked [here](https://forms.gle/pgLSDePCrTxwiDso8).

### Overview&#x20;

**Minimum Spanning Trees.** <mark style="color:red;">Given an undirected graph, a spanning tree T is a subgraph of G, where T is</mark> <mark style="color:red;"></mark><mark style="color:red;">**connected**</mark><mark style="color:red;">,</mark> <mark style="color:red;"></mark><mark style="color:red;">**acyclic**</mark><mark style="color:red;">, includes all vertices</mark>. **The minimum spanning** <mark style="color:red;">tree is the spanning tree whose edge weights have the smallest sum</mark>. MSTs are similar to SPTs, but despite intuition suggesting it may be the case, for many graphs, the MST is not the SPT for any particular vertex. See [https://docs.google.com/presentation/d/1eZ6sCoAY8d-PAfyyTRG\_EQ-BBNqxmTyJ2vS10ZzsGYg/edit#slide=id.g772f8a8e2\_0\_191](https://fa22.datastructur.es/materials/lectures/lec26/thisgraph%20for%20an%20example%20of%20one%20for%20which%20the%20MST%20is%20a%20SPT)

**MST (Minimum Spanning Tree) vs SPT (Shortest Path Tree)**

**Cut Property.** <mark style="color:red;">If you divide the vertices up into two sets S and T (arbitrarily), then a crossing edge is any edge which has one vertex in S and one in T. Neat fact (the cut property): The minimum crossing edge for ANY cut is part of the MST.</mark>

**Prim’s Algorithm.** One approach for finding the MST is as follows: Starting from any arbitrary source, repeatedly add the shortest edge that connects some vertex in the tree to some vertex outside the tree. If the MST is unique, then the result is independent of the source (doesn’t matter where we start).

* Start from some arbitrary start node.
* Repeatedly <mark style="color:red;">add shortest edge (mark black) that has one node inside the MST under construction</mark>.
* Repeat until V-1 edges.

Yet another way of thinking about Prim’s algorithm is that <mark style="color:red;">it is basically just Dijkstra’s algorithm, but where we consider vertices in order of</mark> <mark style="color:red;"></mark><mark style="color:red;">**the distance from the entire tree**</mark><mark style="color:red;">, rather than t</mark><mark style="color:red;">**he distance from the source**</mark>. Or in pseudocode, we simply change relax so that it reads:

```java
relax(e):
    v = e.source
    w = e.target        
    currentBestKnownWeight = distTo(w)
    possiblyBetterWeight = e.weight // Only difference!
    if possiblyBetterWeight > currentBestKnownWeight
        Use e instead of whatever we were using before
```

Demo:[https://docs.google.com/presentation/d/1GPizbySYMsUhnXSXKvbqV4UhPCvrt750MiqPPgU-eCY/edit#slide=id.g9a60b2f52\_0\_0](https://docs.google.com/presentation/d/1GPizbySYMsUhnXSXKvbqV4UhPCvrt750MiqPPgU-eCY/edit#slide=id.g9a60b2f52\_0\_0)

Notice the difference is very subtle! <mark style="color:red;">Like Dijkstra’s, the runtime is O(E log V)</mark>. We can prove that Prim’s works because of the cut property.

**Prim’s vs. Dijkstra’s**

Prim’s and Dijkstra’s algorithms are exactly the same, except Dijkstra’s considers “**distance from the source**”, and Prim’s considers “**distance from the tree**.”

Visit order:

* Dijkstra’s algorithm visits vertices **in order of distance from the source**.
* Prim’s algorithm visits vertices in order of **distance from the MST under construction**.

Relaxation:

* Relaxation in Dijkstra’s considers an edge better based on distance to source.
* Relaxation in Prim’s considers an edge better based on distance to tree.

#### QA

1，为什么会有fringe，直接用distTo不就可以记录和MST的距离了吗？

每次假如新的节点到MST之后，需要更新每个节点到新MST的距离，之后要从中选出距离最小的edge作为下一个加入MST的单位。因此<mark style="color:red;">要使用PQ记录节点做排序（PriorityQueue）</mark>，<mark style="color:red;">计算量为log(n)</mark>；

2，为什么Prim's需要用marked标记MST，而Dijkstra's不需要marked表示SPT？

* <mark style="color:red;">Prim's：marked为1表示该节点属于MST；</mark>
* Dijkstra's：distTo被初始化为infinity，如果从source出发这个节点是可以到达的，就会被赋值一个finite number。因此类似于hasPathTo的function，可以通过distTo来判断；

3，下面代码中的specialPQ是什么意思，和一般的PQ的差别是什么？

从代码看是specialPQ的排序是根据distTo来的，一般PQ的实现方法为根据element的大小排序；<mark style="color:red;">怎是实现specialPQ的？</mark>

```java
// Some code
public class PrimMST {
  public PrimMST(EdgeWeightedGraph G) {
    edgeTo = new Edge[G.V()];
    distTo = new double[G.V()];
    marked = new boolean[G.V()];
    fringe = new SpecialPQ<Double>(G.V());
 
    distTo[s] = 0.0;
    setDistancesToInfinityExceptS(s);
    //Fringe is ordered by distTo tree. Must be a specialPQ like Dijkstra’s.
    insertAllVertices(fringe); 
 
    /* Get vertices in order of distance from tree. */
    while (!fringe.isEmpty()) {
      //Get vertex closest to tree that is unvisited.
      //Important invariant, fringe must be ordered by 
      //current best known distance from tree.
      int v = fringe.delMin();
      //Scan means to consider all of a vertices outgoing edges.
      scan(G, v);
    } 
  }
  
private void scan(EdgeWeightedGraph G, int v) {

  //Vertex is closest, so add to MST.
  marked[v] = true;
  
  for (Edge e : G.adj(v)) {
    int w = e.other(v);
    if (marked[w]) { continue; } //Already in MST, so go to next edge.

    //Better path to a particular vertex found, so update current best 
    //known for that vertex.
    if (e.weight() < distTo[w]) {
      distTo[w] = e.weight();
      edgeTo[w] = e;
      pq.decreasePriority(w, distTo[w]);
    }
  }
}

```

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

**Kruskal’s Algorithm.** As an alternate algorithm and as a showcasing of various data structures in the course, we also considered Kruskal’s algorithm for finding an MST. It performs the exact same task as Prim’s, namely finding an MST, albeit in a different manner. In pseudocode, Kruskal’s algorithm is simply:

```java
Initialize the MST to be empty
Consider each edge e in INCREASING order of weight:
    If adding e to the MST does not result in a cycle, add it to e
```

That’s it! The runtime for Kruskal’s, assuming that we already have all of our edges in a sorted list and use a <mark style="color:red;">**weighted quick union**</mark> with path compression to detect cycles, is <mark style="color:red;">O(E log\*V)</mark>, or (E log E) if we have use a PQ instead. See slides for more details. We can prove that Kruskal’s works because of the cut property.

Completely unimportant technical note: We can actually make an even tighter bound than O(Elog∗V) if we use the inverse Ackermann bound for WQUPC.

### Recommended Problems&#x20;

#### C level&#x20;

1. Problem 2 from [Princeton’s Spring 2008 final](http://www.cs.princeton.edu/courses/archive/fall13/cos226/exams/fin-s08.pdf#page=3)
2. Would Kruskal or Prim’s algorithm work on a directed graph?（No）
3. True or false: Adding a constant to every edge weight does not change the solution to the MST problem (assume unique edge weights).
4. True or false: Multiplying all edges weights with a constant does not change the solution to the MST problem (assume unique edge weights).
5. True or false: It is possible that the only Shortest Path Tree is the only Minimum Spanning Tree.
6. True or false: Prim’s Algorithm and Kruskal’s algorithm will always return the same result.

#### B level&#x20;

1. Adapted from Algorithms 4.3.8. Prove the following, known as the cycle property: Given any cycle in an edge weighted graph (all edge weights distinct), the edge of maximum weight in the cycle does not belong to the MST of the graph.
2. Problem 3 from [Princeton’s Fall 2009 final](http://www.cs.princeton.edu/courses/archive/fall13/cos226/exams/fin-f09.pdf#page=5) (part d is pretty hard).
3. Problem 4 from [Princeton’s Fall 2012 final](http://www.cs.princeton.edu/courses/archive/fall13/cos226/exams/fin-f12.pdf#page=5).
4. Adapted from Algorithms 4.3.12. Suppose that a graph has distinct edge weights. Does its shortest edge have to belong to the MST? Can its longest edge belong to the MST? Does a min-weight edge on every cycle have to belong to the MST? Prove your answer to each question or give a counterexample.
5. Adapted from Algorithms 4.3.20. True or false: At any point during the execution of Kruskal’s algorithm, each vertex is closer to some vertex in its subtree than to any vertex not in its subtree. Prove your answer.
6. True or False: Given any two components that are generated as Kruskal’s algorithm is running (but before it has completed), the smallest edge connecting those two components is part of the MST.
7. Problem 11 from [my Fall 2014 final](http://datastructur.es/sp15/materials/exams/fin-f14.pdf#page=13).
8. Problem 13 from [my fall 2014 final](http://datastructur.es/sp15/materials/exams/fin-f14.pdf#page=15).
9. How would you find the Minimum Spanning Tree where you calculate the weight based off the product of the edges rather than the sum. You may assume that edge weights are >1.

#### A level&#x20;

1. Problem 3 from [Princeton’s Spring 2008 final](http://www.cs.princeton.edu/courses/archive/fall13/cos226/exams/fin-s08.pdf#page=4).
2. Problem 5 from [Kartik’s Algorithm Worksheet](http://www.kartikkapur.com/documents/DataStructureDesign.pdf#page=2).
3. Rigorously prove the following: For any cut C, if the weight of any edge e is smaller than all the other edges across C, then this edge is part of the Minimum Spanning Tree.
4. Adapted from Textbook 4.3.26: An MST edge whose deletion from the graph would cause the MST weight to increase is called a critical edge. Show how to find all critical edges in a graph in time proportional to E log E . Note : This question assumes that edge weights are not necessarily distinct (otherwise all edges in the MST are critical).
