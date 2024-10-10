# Java TreeMap vs HashMap

### **1. Introduction** <a href="#bd-introduction" id="bd-introduction"></a>

In this article, we’re going to compare two _Map_ implementations: _TreeMap_ and _HashMap_.

Both implementations form an integral part of the Java _Collections_ Framework and <mark style="color:red;">store data as</mark> <mark style="color:red;"></mark>_<mark style="color:red;">key-value</mark>_ <mark style="color:red;"></mark><mark style="color:red;">pairs</mark>.

### **2. Differences** <a href="#bd-differences" id="bd-differences"></a>

#### **2.1. Implementation** <a href="#bd-1-implementation" id="bd-1-implementation"></a>

We’ll first talk about the _HashMap_ which is a hashtable-based implementation. It extends the _AbstractMap_ class and implements the _Map_ interface. A _HashMap_ works on the principle of _hashing_.

<mark style="color:red;">This</mark> <mark style="color:red;"></mark>_<mark style="color:red;">Map</mark>_ <mark style="color:red;"></mark><mark style="color:red;">implementation usually acts as a bucketed</mark> <mark style="color:red;"></mark>_<mark style="color:red;">hash table</mark>_<mark style="color:red;">, but</mark> <mark style="color:red;"></mark><mark style="color:red;">**when buckets get too large, they get transformed into nodes of**</mark><mark style="color:red;">** **</mark>_<mark style="color:red;">**TreeNodes**</mark>_<mark style="color:red;">, each structured similarly to those in</mark> <mark style="color:red;"></mark>_<mark style="color:red;">java.util.TreeMap.</mark>_

You can find more on the _HashMap’s_ internals in the article focused on it.

On the other hand, _TreeMap_ extends _AbstractMap_ class and implements _NavigableMap_ interface. <mark style="color:red;">A</mark> <mark style="color:red;"></mark>_<mark style="color:red;">TreeMap</mark>_ <mark style="color:red;"></mark><mark style="color:red;">stores map elements in a</mark> <mark style="color:red;"></mark>_<mark style="color:red;">Red-Black</mark>_ <mark style="color:red;"></mark><mark style="color:red;">tree</mark>, which is a **Self-Balancing **_**Binary Search Tree**_.

And, you can also find more on the _TreeMap’s_ internals in the article focused on it here.

#### **2.2. Order** <a href="#bd-2-order" id="bd-2-order"></a>

_**HashMap**_** doesn’t provide any guarantee over the way the elements are arranged in the **_**Map**_.

It means, **we can’t assume any order while iterating over **_**keys**_** and **_**values**_** of a **_**HashMap**_**:**

```java
@Test
public void whenInsertObjectsHashMap_thenRandomOrder() {
    Map<Integer, String> hashmap = new HashMap<>();
    hashmap.put(3, "TreeMap");
    hashmap.put(2, "vs");
    hashmap.put(1, "HashMap");
    
    assertThat(hashmap.keySet(), containsInAnyOrder(1, 2, 3));
}
```

However, items in a _TreeMap_ are **sorted according to their natural order**.

If _TreeMap_ objects cannot be sorted according to natural order then we may make use of a _Comparator_ or _Comparable_ to define the order in which the elements are arranged within the _Map:_

```java
@Test
public void whenInsertObjectsTreeMap_thenNaturalOrder() {
    Map<Integer, String> treemap = new TreeMap<>();
    treemap.put(3, "TreeMap");
    treemap.put(2, "vs");
    treemap.put(1, "HashMap");
    
    assertThat(treemap.keySet(), contains(1, 2, 3));
}
```

#### **2.3. **_**Null**_** Values** <a href="#bd-3-null-values" id="bd-3-null-values"></a>

_HashMap_ allows storing at most one _null_ _key_ and many _null_ values.

Let’s see an example:

```java
@Test
public void whenInsertNullInHashMap_thenInsertsNull() {
    Map<Integer, String> hashmap = new HashMap<>();
    hashmap.put(null, null);
    
    assertNull(hashmap.get(null));
}
```

However, _TreeMap_ doesn’t allow a _null_ _key_ but may contain many _null_ values.

A _null_ key isn’t allowed because the _compareTo()_ or the _compare()_ method throws a _NullPointerException:_

```java
@Test(expected = NullPointerException.class)
public void whenInsertNullInTreeMap_thenException() {
    Map<Integer, String> treemap = new TreeMap<>();
    treemap.put(null, "NullPointerException");
}
```

**If we’re using a **_**TreeMap**_** with a user-defined **_**Comparator**_**, then it depends on the implementation of the compare**_**()**_** method how **_**null**_** values get handled.**

### **3. Performance Analysis** <a href="#bd-performance-analysis" id="bd-performance-analysis"></a>

Performance is the most critical metric that helps us understand the suitability of a data-structure given a use-case.

In this section, we’ll provide a comprehensive analysis of performance for _HashMap_ and _TreeMap._

#### **3.1. **_**HashMap**_ <a href="#bd-1-hashmap" id="bd-1-hashmap"></a>

_**HashMap,**_** being a hashtable-based implementation, internally uses an array-based data structure to organize its elements according to the **_**hash function**_**.**

_HashMap_ provides expected constant-time performance _<mark style="color:red;">O(1)</mark>_ <mark style="color:red;"></mark><mark style="color:red;">for most operations like</mark> <mark style="color:red;"></mark>_<mark style="color:red;">add()</mark>_<mark style="color:red;">,</mark> <mark style="color:red;"></mark>_<mark style="color:red;">remove()</mark>_ <mark style="color:red;"></mark><mark style="color:red;">and</mark> <mark style="color:red;"></mark>_<mark style="color:red;">contains()</mark>._ Therefore, it’s significantly faster than a _TreeMap_.

The average time to search for an element under the reasonable assumption, in a hash table is _O(1)._ But, an improper implementation of the _hash function_ may lead to a poor distribution of values in buckets which results in:

* Memory Overhead – many buckets remain unused
* Performance Degradation **–** the higher the number of collisions, the lower the performance

**Before Java 8, **_**Separate Chaining**_** was the only preferred way to handle collisions.** It’s usually implemented using linked lists, _i.e._, if there is any collision or two different elements have same hash value then store both the items in the same linked list.

Therefore, searching for an element in a _HashMap,_ in the worst case could have taken as long as searching for an element in a linked list _i.e._ _O(n)_ time.

**However, with** [**JEP 180**](http://openjdk.java.net/jeps/180) **coming into the picture, there’s been a subtle change in the implementation of the way the elements are arranged in a **_**HashMap.**_

According to the specification, when buckets get too large and contain enough nodes they get transformed into modes of _TreeNodes_, each structured similarly to those in _TreeMap_.

<mark style="color:red;">**Hence, in the event of high hash collisions, the worst-case performance will improve from**</mark><mark style="color:red;">** **</mark>_<mark style="color:red;">**O(n)**</mark>_<mark style="color:red;">** **</mark><mark style="color:red;">**to**</mark><mark style="color:red;">** **</mark>_<mark style="color:red;">**O(log n).**</mark>_

The code performing this transformation has been illustrated below:

```java
if(binCount >= TREEIFY_THRESHOLD - 1) {
    treeifyBin(tab, hash);
}
```

The value for _TREEIFY\_THRESHOLD_ is eight which effectively denotes the threshold count for using a tree rather than a linked list for a bucket.

It is evident that:

* A _HashMap_ requires way more memory than is needed to hold its data
* A _HashMap_ shouldn’t be more than 70% – 75% full. If it gets close, it gets resized and entries rehashed
* Rehashing requires _n_ operations which is costly wherein our constant time insert becomes of order _O(n)_
* It’s the hashing algorithm which determines the order of inserting the objects in the _HashMap_

**The performance of a **_**HashMap**_** can be tuned by setting the custom **_**initial capacity**_** and the **_**load factor**_, at the time of _HashMap_ object creation itself.

However, we should choose a _HashMap_ if:

* we know approximately how many items to maintain in our collection
* we don’t want to extract items in a natural order

Under the above circumstances, _HashMap_ is our best choice because it offers constant time insertion, search, and deletion.

#### **3.2. **_**TreeMap**_ <a href="#bd-2-treemap" id="bd-2-treemap"></a>

A _TreeMap_ stores its data in a hierarchical tree with the ability to sort the elements with the help of a custom _Comparator._

A summary of its performance:

* _TreeMap_ provides a performance of _O(log(n))_ for most operations like _add()_, _remove()_ and _contains()_
* A _Treemap_ can save memory (in comparison to _HashMap)_ because it only uses the amount of memory needed to hold its items, unlike a _HashMap_ which uses contiguous region of memory
* A tree should maintain its balance in order to keep its intended performance, this requires a considerable amount of effort, hence complicates the implementation

We should go for a _TreeMap_ whenever:

* memory limitations have to be taken into consideration
* we don’t know how many items have to be stored in memory
* we want to extract objects in a natural order
* if items will be consistently added and removed
* we’re willing to accept _O(log n)_ search time

### **4. Similarities** <a href="#bd-introduction-1" id="bd-introduction-1"></a>

#### **4.1. Unique Elements** <a href="#bd-introduction-2" id="bd-introduction-2"></a>

Both _TreeMap_ and _HashMap_ don’t support duplicate keys. If added, it overrides the previous element (without an error or an exception):

```java
@Test
public void givenHashMapAndTreeMap_whenputDuplicates_thenOnlyUnique() {
    Map<Integer, String> treeMap = new HashMap<>();
    treeMap.put(1, "Baeldung");
    treeMap.put(1, "Baeldung");

    assertTrue(treeMap.size() == 1);

    Map<Integer, String> treeMap2 = new TreeMap<>();
    treeMap2.put(1, "Baeldung");
    treeMap2.put(1, "Baeldung");

    assertTrue(treeMap2.size() == 1);
}
```

#### **4.2. Concurrent Access** <a href="#bd-introduction-3" id="bd-introduction-3"></a>

**Both **_**Map**_** implementations aren’t **_**synchronized**_ and we need to manage concurrent access on our own.

Both must be synchronized externally whenever multiple threads access them concurrently and at least one of the threads modifies them.

We have to explicitly use _Collections.synchronizedMap(mapName)_ to obtain a synchronized view of a provided map.

#### **4.3. Fail-Fast Iterators** <a href="#bd-introduction-4" id="bd-introduction-4"></a>

The _Iterator_ throws a _ConcurrentModificationException_ if the _Map_ gets modified in any way and at any time once the iterator has been created.

Additionally, we can use the iterator’s remove method to alter the _Map_ during iteration.

Let’s see an example:

```java
@Test
public void whenModifyMapDuringIteration_thenThrowExecption() {
    Map<Integer, String> hashmap = new HashMap<>();
    hashmap.put(1, "One");
    hashmap.put(2, "Two");
    
    Executable executable = () -> hashmap
      .forEach((key,value) -> hashmap.remove(1));
 
    assertThrows(ConcurrentModificationException.class, executable);
}
```

### **5. Which Implementation to Use?** <a href="#bd-introduction-5" id="bd-introduction-5"></a>

In general, both implementations have their respective pros and cons, however, **it’s about understanding the underlying expectation and requirement which must govern our choice regarding the same.**

Summarizing:

* We should use a _TreeMap_ if we want to keep our entries sorted
* We should use a _HashMap_ if we prioritize performance over memory consumption
* Since a _TreeMap_ has a more significant locality, we might consider it if we want to access objects that are relatively close to each other according to their natural ordering
* _HashMap_ can be tuned using the _initialCapacity_ and _loadFactor_, which isn’t possible for the _TreeMap_
* We can use the _LinkedHashMap_ if we want to preserve insertion order while benefiting from constant time access

### **6. Conclusion** <a href="#bd-introduction-6" id="bd-introduction-6"></a>

In this article, we showed the differences and similarities between _TreeMap_ and _HashMap_.

As always, the code examples for this article are available [over on GitHub](https://github.com/eugenp/tutorials/tree/master/core-java-modules/core-java-collections-maps-3).
