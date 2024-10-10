# Java HashSet and TreeSet

### **1. Introduction** <a href="#bd-introduction" id="bd-introduction"></a>

In this article, we’re going to compare two of the most popular Java implementations of the _java.util.Set_ interface – _HashSet_ and _TreeSet_.

### **2. Differences** <a href="#bd-introduction-1" id="bd-introduction-1"></a>

_HashSet_ and _TreeSet_ are leaves of the same branch, but they differ in few important matters.

#### **2.1. Ordering** <a href="#bd-introduction-2" id="bd-introduction-2"></a>

_**HashSet**_** stores the objects in random order, whereas **_**TreeSet**_** applies the natural order of the elements.** Let’s see the following example:

```java
@Test
public void givenTreeSet_whenRetrievesObjects_thenNaturalOrder() {
    Set<String> set = new TreeSet<>();
    set.add("Baeldung");
    set.add("is");
    set.add("Awesome");
 
    assertEquals(3, set.size());
    assertTrue(set.iterator().next().equals("Awesome"));
}
```

After adding the _String_ objects into _TreeSet_, we see that the first one is “Awesome”, even though it was added at the very end. A similar operation done with _HashSet_ does not guarantee that the order of elements will remain constant over time.

#### **2.2. **_**Null**_** Objects** <a href="#bd-introduction-3" id="bd-introduction-3"></a>

Another difference is that _**HashSet**_** can store **_**null**_** objects, while **_**TreeSet**_** does not allow them**:

```java
@Test(expected = NullPointerException.class)
public void givenTreeSet_whenAddNullObject_thenNullPointer() {
    Set<String> set = new TreeSet<>();
    set.add("Baeldung");
    set.add("is");
    set.add(null);
}

@Test
public void givenHashSet_whenAddNullObject_thenOK() {
    Set<String> set = new HashSet<>();
    set.add("Baeldung");
    set.add("is");
    set.add(null);
 
    assertEquals(3, set.size());
}
```

If we try to store the _null_ object in a _TreeSet_, the operation will result in a thrown _NullPointerException_. The only exception was in Java 7 when it was allowed to have exactly one _null_ element in the _TreeSet_.

#### **2.3. Performance** <a href="#bd-introduction-4" id="bd-introduction-4"></a>

**Simply put, **_**HashSet**_** is faster than the **_**TreeSet**_**.**

_HashSet_ provides constant-time performance for most operations like _add()_, _remove()_ and _contains()_, versus the _log_(_n_) time offered by the _TreeSet._

Usually, we can see that **the execution time for adding elements into **_**TreeSet**_** is much more than for the **_**HashSet**_.

Please remember that the JVM might be not warmed up, so the execution times can differ. A good discussion how to design and perform micro tests using various _Set_ implementations is available [here](http://stackoverflow.com/questions/23168490/hashset-and-treeset-performance-test).

#### **2.4. Implemented Methods** <a href="#bd-introduction-5" id="bd-introduction-5"></a>

_**TreeSet**_** is rich in functionalities**, implementing additional methods like:

* _**pollFirst()**_ – to return the first element, or _null_ if _Set_ is empty
* _**pollLast()**_ – to retrieve and remove the last element, or return _null_ if _Set_ is empty
* _**first()**_ – to return the first item
* _**last()**_** –** to return the last item
* _**ceiling()**_ – to return the least element greater than or equal to the given element, or _null_ if there is no such element
* _**lower()**_ – to return the largest element strictly less than the given element, or _null_ if there is no such element

The methods mentioned above make _TreeSet_ much easier to use and more powerful than _HashSet_.

### **3. Similarities** <a href="#bd-introduction-6" id="bd-introduction-6"></a>

#### **3.1. Unique Elements** <a href="#bd-introduction-7" id="bd-introduction-7"></a>

Both _TreeSet_ and _HashSet_ guarantee a **duplicate-free collection of elements,** as it is a part of the generic _Set_ interface:

```java
@Test
public void givenHashSetAndTreeSet_whenAddDuplicates_thenOnlyUnique() {
    Set<String> set = new HashSet<>();
    set.add("Baeldung");
    set.add("Baeldung");
 
    assertTrue(set.size() == 1);
        
    Set<String> set2 = new TreeSet<>();
    set2.add("Baeldung");
    set2.add("Baeldung");
 
    assertTrue(set2.size() == 1);
}
```

#### **3.2. Not **_**synchronized**_ <a href="#bd-introduction-8" id="bd-introduction-8"></a>

**None of the described **_**Set**_** implementations are **_**synchronized**_**.** This means that if multiple threads access a _Set_ concurrently, and at least one of the threads modifies it, then it must be synchronized externally.

#### **3.3. Fail-fast Iterators** <a href="#bd-introduction-9" id="bd-introduction-9"></a>

**The **_**Iterator**_**s returned by **_**TreeSet**_** and **_**HashSet**_** are fail-fast.**

That means that any modification of the _Set_ at any time after the _Iterator_ is created will throw a _ConcurrentModificationException:_

```java
@Test(expected = ConcurrentModificationException.class)
public void givenHashSet_whenModifyWhenIterator_thenFailFast() {
    Set<String> set = new HashSet<>();
    set.add("Baeldung");
    Iterator<String> it = set.iterator();

    while (it.hasNext()) {
        set.add("Awesome");
        it.next();
    }
}
```

### **4. Which Implementation to Use?** <a href="#bd-introduction-10" id="bd-introduction-10"></a>

Both implementations fulfill the contract of the idea of a set so it’s up to the context which implementation we might use.

Here are few quick points to remember:

* If we want to keep our entries sorted, we need to go for the _TreeSet_
* If we value performance more than memory consumption, we should go for the _HashSet_
* If we are short on memory, we should go for the _TreeSet_
* If we want to access elements that are relatively close to each other according to their natural ordering, we might want to consider _TreeSet_ because it has greater locality
* _HashSet_‘s performance can be tuned using the _initialCapacity_ and _loadFactor_, which is not possible for the _TreeSet_
* If we want to preserve insertion order and benefit from constant time access, we can use the _LinkedHashSet_

### **5. Conclusion** <a href="#bd-introduction-11" id="bd-introduction-11"></a>

In this article, we covered the differences and similarities between _TreeSet_ and _HashSet_.

As always, the code examples for this article are available [over on GitHub](https://github.com/eugenp/tutorials/tree/master/core-java-modules/core-java-collections-set).
