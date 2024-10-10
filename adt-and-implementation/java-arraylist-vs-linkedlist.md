# Java ArrayList vs LinkedList

### 1. Overview <a href="#bd-overview" id="bd-overview"></a>

When it comes to collections, the Java standard library provides plenty of options to choose from. Among those options are two famous _List_ implementations known as _ArrayList_ and _LinkedList,_ each with their own properties and use-cases.

In this tutorial, we’re going to see how these two are actually implemented. Then, we’ll evaluate different applications for each one.

Internally, _**ArrayList**_** is using an array to implement the **_**List**_** interface**. As arrays are fixed size in Java, _ArrayList_ creates an array with some initial capacity. Along the way, if we need to store more items than that default capacity, it will replace that array with a new and more spacious one.

To better understand its properties, let’s evaluate this data structure with respect to its three main operations: [adding items](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/List.html#add\(E\)), [getting one by index](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/List.html#get\(int\)) and [removing by index](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayList.html#remove\(int\)).

#### 2.1. Add <a href="#bd-1-add" id="bd-1-add"></a>

When we’re creating an empty _ArrayList,_ it initializes its backing array with a [default capacity](https://github.com/openjdk/jdk/blob/827e5e32264666639d36990edd5e7d0b7e7c78a9/src/java.base/share/classes/java/util/ArrayList.java#L118) (currently 10):

<figure><img src="../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

Adding a new item while that array it not yet full is as simple as assigning that item to a specific array index. This array index is determined by the current array size since we’re practically appending to the list:

```java
backingArray[size] = newItem;
size++;
```

So, **in best and average cases, the time complexity for the add operation is **_**O(1)**,_ which is pretty fast. As the backing array becomes full, however, the add implementation becomes less efficient:

<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

**To add a new item, we should first initialize a brand new array with** [**more capacity**](https://github.com/openjdk/jdk/blob/827e5e32264666639d36990edd5e7d0b7e7c78a9/src/java.base/share/classes/java/util/ArrayList.java#L230) **and copy all existing items to the new array.** Only after copying current elements can we add the new item. Hence, the time complexity is _O(n)_ in the worst case since we have to copy _n_ elements first.

**Theoretically speaking, adding a new element runs in** [**amortized**](https://en.wikipedia.org/wiki/Amortized\_analysis) **constant time. That is, adding **_**n**_** elements requires **_**O(n)**_** time. However, some single additions may perform poorly because of the copy overhead.**&#x20;

#### 2.2. Access by Index <a href="#bd-2-access-by-index" id="bd-2-access-by-index"></a>

Accessing items by their indices is where the _ArrayList_ really shines. To retrieve an item at index _i,_ we just have to return the item residing at the _ith_ index from the backing array. Consequently, **the time complexity for access by index operation is always **_**O(1).**_&#x20;

#### 2.3. Remove by Index <a href="#bd-3-remove-by-index" id="bd-3-remove-by-index"></a>

Suppose we’re going to remove the index 6 from our _ArrayList,_ which corresponds to the element 15 in our backing array:

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

After marking the desired element as deleted, we should move all elements after it back by one index. **Obviously, the nearer the element to the start of the array, the more elements we should move.** So the time complexity is _O(1)_ at the best-case and _O(n)_ on average and worst-cases.

#### 2.4. Applications and Limitations <a href="#bd-4-applications-and-limitations" id="bd-4-applications-and-limitations"></a>

Usually, _ArrayList_ is the default choice for many developers when they need a _List_ implementation. As a matter of fact, **it’s actually a sensible choice when the number of reads is far more than the number of writes**.

Sometimes we need equally frequent reads and writes. **If we do have an estimate of the maximum number of possible items, then it still makes sense to use **_**ArrayList**_. If that’s the case, we can initialize the _ArrayList_ with an initial capacity:

```java
int possibleUpperBound = 10_000;
List<String> items = new ArrayList<>(possibleUpperBound);
```

This estimation may prevent lots of unnecessary copying and array allocations.

Moreover, **arrays are indexed by** [_**int**_** values**](https://docs.oracle.com/javase/specs/jls/se13/html/jls-10.html#jls-10.4) **in Java. So, it’s not possible to store more than **_**232**_** elements in a Java array and, consequently, in **_**ArrayList**_.

### 3. _LinkedList_ <a href="#bd-linked-list" id="bd-linked-list"></a>

_LinkedList_, as its name suggests, **uses a collection of linked nodes to store and retrieve elements**. For instance, here’s how the Java implementation looks after adding four elements:

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

**Each node maintains two pointers: one pointing to the next element and another referring to the previous one. Expanding on this, the** [**doubly linked list**](https://en.wikipedia.org/wiki/Doubly\_linked\_list) **has two pointers pointing to the first and last items.**

Again, let’s evaluate this implementation with respect to the same fundamental operations.

#### 3.1. Add <a href="#bd-1-add-1" id="bd-1-add-1"></a>

In order to add a new node, first, we should link the current last node to the new node:

<figure><img src="../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

And then update the last pointer:

<figure><img src="../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

**As both of these operations are trivial, the time complexity for the add operation is always **_**O(1)**_**.**

#### 3.2. Access by Index <a href="#bd-2-access-by-index-1" id="bd-2-access-by-index-1"></a>

_**LinkedList,**_** as opposed to **_**ArrayList,**_** does not support fast random access. So, in order to find an element by index, we should traverse some portion of the list** **manually**.

In the best case, when the requested item is near the start or end of the list, the time complexity would be as fast as _O(1)._ However, in the average and worst-case scenarios, we may end up with an _O(n)_ access time since we have to examine many nodes one after another.

#### 3.3. Remove by Index <a href="#bd-3-remove-by-index-1" id="bd-3-remove-by-index-1"></a>

**In order to** [**remove**](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/LinkedList.html#remove\(int\)) **an item, we should first find the requested item and then un-link it from the list**. Consequently, the access time determines the time complexity — that is, _O(1)_ at best-case and _O(n)_ on average and in worst-case scenarios.

#### 3.4. Applications <a href="#bd-4-applications" id="bd-4-applications"></a>

_**LinkedLists**_** are more suitable when the addition rate is much higher than the read rate**.

Also, it can be used in read-heavy scenarios when most of the time we want the first or last element. It’s worth mentioning that _LinkedList_ also implements the [_Deque_](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Deque.html) interface –  supporting efficient access to both ends of the collection.

Generally, if we know their implementation differences, then we could easily choose one for a particular use-case.

For instance, let’s say that we’re going store a lot of time-series events in a list-like data structure. We know that we would receive bursts of events each second.

Also, we need to examine all events one after another periodically and provide some stats. For this use-case, _LinkedList_ is a better choice because the addition rate is much higher than the read rate.

Also, we would read all the items, so we can’t beat the _O(n)_ upper bound.

### 4. Conclusion <a href="#bd-conclusion" id="bd-conclusion"></a>

In this tutorial, first, we took a dive into how _ArrayList_ and _LinkLists_ are implemented in Java.

We also evaluated different use-cases for each one of these.
