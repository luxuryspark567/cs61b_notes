---
description: https://www.topcoder.com/thrive/articles/ternary-search-trees
---

# Ternary Search Tree

Animation:

[https://www.cs.usfca.edu/\~galles/visualization/TST.html](https://www.cs.usfca.edu/\~galles/visualization/TST.html)

Ternary search trees are a similar data structure to binary search trees and tries. They encapsulate the memory efficiency of binary search trees and time efficiency of tries.

We know that by using tries we can search and sort strings efficiently, but it consumes a lot of memory. Here we can use ternary search trees instead, which store fewer references and null objects.

Characteristics of **Ternary Search Trees (TST)**

* TST stores characters or strings in nodes
* Each node has three children: left, equal, right
* The left pointer: smaller than the parent node;
* The equal pointer: <mark style="color:red;">the next character</mark> in a given word;
* The right pointer: greater than the parent node;

![image](https://images.contentful.com/piwi0eufbb2g/oAztgp79Z1dhuvV7cQQw8/19c74ee1250af0f1eff89d03b3ba0d68/image.png)

Ternary search trees declaration

```java
struct TSTNode {
  char data;
  int isEnd;
  TSTNode * left;
  TSTNode * equal;
  TSTNode * right;
};
```

![image](https://images.contentful.com/piwi0eufbb2g/AwT8SMzqwsJQAfyi0zyrA/d89bb81e06aa796ff15c077d74c74078/image.png)

In general, we have as many pointers/edges from every node as the number of characters in the alphabet.\
We have to define an alphabet in advance + ALP\_SIZE. For example: in the English alphabet, there are 26 characters, so ALP\_SIZE = 26 -> 26 pointers from every node.

#### Inserting string in the TST:

To explain it further, we will try to insert some strings in the TST: boats, boat, bat, bats.

We will insert in this order:

First, let’s insert "boats".

<figure><img src=".gitbook/assets/image (121).png" alt="" width="50"><figcaption></figcaption></figure>

Now to insert "boat" to our TST we just have to mark isEnd of of “t” to 1.

<figure><img src=".gitbook/assets/image (122).png" alt="" width="50"><figcaption></figcaption></figure>

Now, let’s insert "_bat"_.

<figure><img src=".gitbook/assets/image (123).png" alt="" width="138"><figcaption></figcaption></figure>

Now, at last, insert "_bats"_.

<figure><img src=".gitbook/assets/image (124).png" alt="" width="138"><figcaption></figcaption></figure>

**Code for inserting into TST:**

```java
TSTNode * insertInTST(TSTNode * root, char * word) {

  if (root == NULL) {
    root = new TSTNode();
    root - > data = * word;
    root - > isEnd = 1;
    root - > left = root - > equal = root - > right = NULL;
  }

  if ( * word < root - > data)
    root - > left = insertInTST(root - > left, word);
  else if ( * word == root - > data) {
    if ( * (word + 1))
      root - > equal = insertInTST(root - > equal, word + 1);
    else
      root - > isEnd = 1;
  } else
    root - > right = insertInTST(root - > right, word);
  return root;
}
```

Time Complexity: O(N), where N is the length of the string to be inserted.

关于TST：

* Tree中有多少个深色节点，就有多少个Node；
* search：如果是往中间走，就要减少一个char往下；如果是往两边就不能减少，并且继续搜索；搜索"boat"，在"o"往下接下来搜索的是"at"，相当于把o吃掉了；搜索"bat"，在"o"往左，"a"没有被吃掉，因为"a" < "o"，往左边走。

<figure><img src=".gitbook/assets/image (125).png" alt="" width="252"><figcaption></figcaption></figure>



#### Searching string in the TST:

While searching we can follow the same procedure as the binary search tree. The only difference is that in cases when the character matches we check for the remaining characters instead of returning.

**Code for searching into TST:**

```java
int searchInTST(TSTNode * root, char * word) {

  if (!root)
    return -1;
  if ( * word < root - > data)
    return searchInTST(root - > left, word);
  else if ( * word > root - > data)
    return searchInTST(root - > right, word);
  else {
    if (root - > isEnd && * (word + 1) == 0)
      return 1;
    return searchInTST(root - > eq, word + 1);
  }
}
```

Time Complexity: O(N), where N is the length of the string to be searched.

#### TST vs Hashing

**TST**

* Works for strings
* Examines just enough key characters
* May only miss a few characters
* Supports much more operation
* More flexible than binary search trees
* Faster than hashing

**Hashing**

* Needs to examine the entire key
* Search hits and misses cost the same
* Run time and performance relies heavily on the hash function
* Does not support as much operation as TST

**Applications of TST**

* It can be used to implement the auto-complete feature efficiently
* Can be used for spell checkers
* Near neighbor searching
* For databases, especially when indexing by several non-key fields.
