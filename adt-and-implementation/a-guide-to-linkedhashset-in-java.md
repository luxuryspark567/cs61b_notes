# A Guide to LinkedHashSet in Java

### **1. Overview** <a href="#bd-overview" id="bd-overview"></a>

In this article, **we’ll explore the** [_**LinkedHashSet**_](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/LinkedHashSet.html) **class of the Java **_**Collections**_** API**. We’ll dive into the features of this data structure and demonstrate its functionalities.

### **2. Introduction to **_**LinkedHashSet**_ <a href="#bd-introduction-to-linkedhashset" id="bd-introduction-to-linkedhashset"></a>

The _LinkedHashSet_ is a generic data structure that belongs to the _Java.util_ library. It’s a direct descendant of the _HashSet_ data structure, hence, contains non-duplicate elements at every given time.

**In addition to having a doubly-linked list running through all of its entries, its implementation is different from that of the **_**HashSet**_** in that it maintains a predictable iteration order**. The iteration order is defined by the insertion order of the elements into the set.

**The **_**LinkedHashSet**_** saves the client from the unpredictable ordering provided by **_**HashSet**_**, without incurring the complexity that comes with a **_**TreeSet**_.

While **its performance is likely to be just slightly less than that of **_**HashSet**_, due to the added expense of maintaining the linked list, **it has constant-time (O1) performance for **_**add(), contains()**_** and **_**remove()**_** operations**.

### 3. Create a _LinkedHashSet_ <a href="#bd-create-a-linkedhashset" id="bd-create-a-linkedhashset"></a>

There are several constructors available to create a _LinkedHashSet_. Let’s have a look at each one of them:

#### 3.1. Default No-Arg Constructor <a href="#bd-1-default-no-arg-constructor" id="bd-1-default-no-arg-constructor"></a>

```
Set<String> linkedHashSet = new LinkedHashSet<>();
assertTrue(linkedHashSet.isEmpty());
```

#### 3.2. Create with an Initial Capacity <a href="#bd-2-create-with-an-initial-capacity" id="bd-2-create-with-an-initial-capacity"></a>

The initial capacity represents the initial length of the _LinkedHashSet_. **Providing an initial capacity prevents any unnecessary resizing of the **_**Set**_** as it grows**. The default initial capacity is 16:

```
LinkedHashSet<String> linkedHashSet = new LinkedHashSet<>(20);
```

#### 3.3. Create from a _Collection_ <a href="#bd-3-create-from-a-collection" id="bd-3-create-from-a-collection"></a>

We can also use the content of a _Collection_ to populate a _LinkedHashSet_ object at the point of creation:

```
@Test
 void whenCreatingLinkedHashSetWithExistingCollection_shouldContainAllElementOfCollection(){
      Collection<String> data = Arrays.asList("first", "second", "third", "fourth", "fifth");
      LinkedHashSet<String> linkedHashSet = new LinkedHashSet<>(data);

      assertFalse(linkedHashSet.isEmpty());
      assertEquals(data.size(), linkedHashSet.size());
      assertTrue(linkedHashSet.containsAll(data) && data.containsAll(linkedHashSet));
 }
```

#### 3.4. Create with Initial Capacity and Load Factor <a href="#bd-4-create-with-initial-capacity-and-load-factor" id="bd-4-create-with-initial-capacity-and-load-factor"></a>

When the size of the _LinkedHashSet_ grows to exceed the value of the initial capacity, the new capacity is the multiplication of the load factor and the previous capacity. In the below snippet, the initial capacity is set to 20 and the load factor is 3.

```
LinkedHashSet<String> linkedHashSet = new LinkedHashSet<>(20, 3);
```

The default load factor is 0.75.

### 4. Adding an Element to the _LinkedHashSet_ <a href="#bd-adding-an-element-to-the-linkedhashset" id="bd-adding-an-element-to-the-linkedhashset"></a>

We can either add a single element or a _Collection_ of elements to the _LinkedHashSet_ by using the _add()_ and _addAll()_ methods respectively.  **An element will be added if it doesn’t already exist in the **_**Set**_. **These methods return **_**true**_** when an element is added to the set, otherwise, they return **_**false**_.

#### 4.1. Add a Single Element <a href="#bd-1-add-a-single-element" id="bd-1-add-a-single-element"></a>

Here’s an implementation to add an element to a _LinkedHashSet_:

```
@Test
void whenAddingElement_shouldAddElement(){
    Set<Integer> linkedHashSet = new LinkedHashSet<>();
    assertTrue(linkedHashSet.add(0));
    assertFalse(linkedHashSet.add(0));
    assertTrue(linkedHashSet.contains(0));

}
```

#### 4.2. Add a _Collection_ of Elements <a href="#bd-2-add-a-collection-of-elements" id="bd-2-add-a-collection-of-elements"></a>

As mentioned earlier, we can also add a _Collection_ of elements to a _LinkedHashSet_:

```
@Test
void whenAddingCollection_shouldAddAllContentOfCollection(){
    Collection<Integer> data = Arrays.asList(1,2,3);
    LinkedHashSet<Integer> linkedHashSet = new LinkedHashSet<>();

    assertTrue(linkedHashSet.addAll(data));
    assertTrue(data.containsAll(linkedHashSet) && linkedHashSet.containsAll(data));
 }
```

**The rule of not adding duplicates also applies to the **_**addAll()**_** method** as demonstrated below:

```
@Test
void whenAddingCollectionWithDuplicateElements_shouldMaintainUniqueValuesInSet(){
    LinkedHashSet<Integer> linkedHashSet = new LinkedHashSet<>();
    linkedHashSet.add(2);
    Collection<Integer> data = Arrays.asList(1, 1, 2, 3);

    assertTrue(linkedHashSet.addAll(data));
    assertEquals(3, linkedHashSet.size());
    assertTrue(data.containsAll(linkedHashSet) && linkedHashSet.containsAll(data));
}
```

Notice that the _data_ variable contains duplicate values of 1 and the _LinkedHashSet_ already contains the _Integer_ value 2 before invoking the _addAll()_ method.

### 5. Iterating Through a _LinkedHashSet_ <a href="#bd-iterating-through-a-linkedhashset" id="bd-iterating-through-a-linkedhashset"></a>

Like every other descendant of the _Collection_ library, we can iterate through a _LinkedHashSet_. **Two types of iterators are available in a **_**LinkedHashSet**_**: **_**Iterator**_** and **_**Spliterator**_.

**While the former is only able to traverse and perform any basic operation on a **_**Collection**_**, the latter splits the **_**Collection**_** into subsets and performs different operations on each subset in parallel, thereby making it thread-safe**.

#### 5.1. Iterating with an _Iterator_ <a href="#bd-1-iterating-with-an-iterator" id="bd-1-iterating-with-an-iterator"></a>

```
@Test
void whenIteratingWithIterator_assertThatElementIsPresent(){
    LinkedHashSet<Integer> linkedHashSet = new LinkedHashSet<>();
    linkedHashSet.add(0);
    linkedHashSet.add(1);
    linkedHashSet.add(2);

    Iterator<Integer> iterator = linkedHashSet.iterator();
    for (int i = 0; i < linkedHashSet.size(); i++) {
        int nextData = iterator.next();
        assertEquals(i, nextData);
    }
}
```

#### 5.2. Iterating with a _Spliterator_ <a href="#bd-2-iterating-with-a-spliterator" id="bd-2-iterating-with-a-spliterator"></a>

```
@Test
void whenIteratingWithSpliterator_assertThatElementIsPresent(){
    LinkedHashSet<Integer> linkedHashSet = new LinkedHashSet<>();
    linkedHashSet.add(0);
    linkedHashSet.add(1);
    linkedHashSet.add(2);

    Spliterator<Integer> spliterator = linkedHashSet.spliterator();
    AtomicInteger counter = new AtomicInteger();
    spliterator.forEachRemaining(data -> {
       assertEquals(counter.get(), (int)data);
       counter.getAndIncrement();
    });
}
```

### 6. Remove Elements from the _LinkedHashSet_ <a href="#bd-remove-elements-from-the-linkedhashset" id="bd-remove-elements-from-the-linkedhashset"></a>

Here are the different ways to remove an element from the _LinkedHashSet_:

#### 6.1. _remove()_ <a href="#bd-1-remove" id="bd-1-remove"></a>

This method removes an element from the _Set_ given that we know the exact element we want to remove. It accepts an argument that’s the actual element we want to remove and returns _true_ if successfully removed, otherwise _false_:

```
@Test
void whenRemovingAnElement_shouldRemoveElement(){
    Collection<String> data = Arrays.asList("first", "second", "third", "fourth", "fifth");
    LinkedHashSet<String> linkedHashSet = new LinkedHashSet<>(data);

    assertTrue(linkedHashSet.remove("second"));
    assertFalse(linkedHashSet.contains("second"));
}
```

#### 6.2. _removeIf()_ <a href="#bd-2-removeif" id="bd-2-removeif"></a>

**The **_**removeIf()**_** method removes an element that satisfies specified predicate conditions.** The example below removes all elements in the _LinkedHashSet_ that are greater than 2:

```
@Test
void whenRemovingAnElementGreaterThanTwo_shouldRemoveElement(){
    LinkedHashSet<Integer> linkedHashSet = new LinkedHashSet<>();
    linkedHashSet.add(0);
    linkedHashSet.add(1);
    linkedHashSet.add(2);
    linkedHashSet.add(3);
    linkedHashSet.add(4);

    linkedHashSet.removeIf(data -> data > 2);
    assertFalse(linkedHashSet.contains(3));
    assertFalse(linkedHashSet.contains(4));
}
```

#### 6.3. Removing with an _Iterator_ <a href="#bd-3-removing-with-an-iterator" id="bd-3-removing-with-an-iterator"></a>

The _iterator_ is also another option we can use to remove elements from the _LinkedHashSet_. The _remove()_ method of the _Iterator_ removes the element that the _Iterator_ is currently on:

```
@Test
void whenRemovingAnElementWithIterator_shouldRemoveElement(){
    LinkedHashSet<Integer> linkedHashSet = new LinkedHashSet<>();
    linkedHashSet.add(0);
    linkedHashSet.add(1);
    linkedHashSet.add(2);

    Iterator<Integer> iterator = linkedHashSet.iterator();
    int elementToRemove = 1;
    assertTrue(linkedHashSet.contains(elementToRemove));
    while(iterator.hasNext()){
        if(elementToRemove == iterator.next()){
           iterator.remove();
       }
    }
    assertFalse(linkedHashSet.contains(elementToRemove));
}
```

### 7. Conclusion <a href="#bd-conclusion" id="bd-conclusion"></a>

In this article, we studied the _LinkedHashSet_ data structure from the Java _Collections_ library. We demonstrated how to create a _LinkedHashSet_ through its different constructors, adding and removing elements, as well as iterating through it. We also, learned the underlying layers of this data structure, its advantage over the _HashSet_ as well as the time complexity for its common operations.

As always, code snippets can be found [over on GitHub](https://github.com/eugenp/tutorials/tree/master/core-java-modules/core-java-collections-set-2).
