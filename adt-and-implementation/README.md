---
description: 总结ADT和不同的Implementation
---

# ADT and Implementation

<figure><img src="../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

```java
// Using ADT
List<String> hl = new ArrayList<>();
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

**ADT**:&#x20;

* **List**
* **Set**
* **Map**
* **DisjointSets**
* **PQ**
* **Stack**

**Implementation**:&#x20;

* **List**: Linked List, Array List
* **Tree**: BST, 2-3 Tree, LLRB
* **Hash Table**: Separate Chaining, Linear Probing
* **Heap**

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>
