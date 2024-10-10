# CS61B Sorting Cheat Sheet

Robert Lu

## **Basic Consepts**

**Inversions.** The number of pairs of elements in a sequence <mark style="color:red;">that are out of order</mark>. An array with no inversions is ordered.

## **Sorting Summary**&#x20;

Listed by mechanism:

* **Selection sort**: Find the smallest item and put it at the front (left to right).
* **Insertion sort**: Like poker, figure out where to insert the current item (left to right).
* **Merge sort**: Merge two sorted halves into one sorted whole.&#x20;
* **Partition sort  (Quick sort)** : Partition items around a pivot.

### **Selection sort**

**Native Selection Sort**: Find the smallest item and put it at the front (left to right), takes Θ(N2) time, Demo: [https://goo.gl/g14Cit](https://goo.gl/g14Cit)

**Naive Heapsort.** A variant of selection sort is to use a heap based PQ to sort the items. To do this, insert all items into a <mark style="color:red;">MaxPQ</mark> and then remove them one by one. The first such item removed is placed at the end of the array, the next item right before the end, and so forth until that last item deleted is placed in position 0 of the array. Each insertion and deletion takes <mark style="color:red;">O(log N)</mark> time, and there are N insertions and deletions, resulting in a <mark style="color:red;">O(N log N)</mark> runtime. With some more work, we can show that heapsort is Θ(N log N) in the worst case. This naive version of heapsort uses Θ(N) for the PQ. Creation of the MaxPQ requires O(N) memory. It is also possible to use a MinPQ instead.

**In Place Heapsort.** When sorting an array, we can avoid the Θ(N) memory cost by treating the array itself as a heap. To do this,&#x20;

* Heapify the array using <mark style="color:red;">bottom-up heap</mark> construction (taking Θ<mark style="color:red;">(N)</mark> time)
  * Sink nodes in reverse level order: sink(k)
  * After sinking, guaranteed that tree rooted at position k is a heap.
* Repeatedly delete the max item, swapping it with the last item in the heap.&#x20;
* In-place heap sort: [Demo](https://docs.google.com/presentation/d/1SzcQC48OB9agStD0dFRgccU-tyjD6m3esrSC-GLxmNc/edit?usp=sharing).

Over time, the heap shrinks from N items to 0 items, and the sorted list from 0 items to N items. The resulting version is also <mark style="color:red;">Θ(N log N)</mark>.

### **Insertion Sort**

**Like poker**. For each item, insert into the output sequence in the appropriate place. Every swap fixes exactly one inversion. In the best case, <mark style="color:red;">insertion sort takes Θ(N) time. In the worst case, Θ(N^2) time</mark>. More generally, **runtime is no worse than the number of inversions**.

**Inplace Insertion Sort**&#x20;

General strategy:&#x20;

* Repeat for i = 0 to N - 1:
  * Designate item i as the traveling item.
  * Swap item backwards until traveller is in the right place among all previously examined items.
  * In-place using swapping: [Demo](https://docs.google.com/presentation/d/10b9aRqpGJu8pUk8OpfqUIEEm8ou-zmmC7b\_BE5wgNg0/edit?usp=sharing)

**To Keep In Mind:**

* <mark style="color:red;">For small arrays (N < 15 or so), insertion sort is fastest</mark>. (Rough idea: Divide and conquer algorithms like heapsort / mergesort spend too much time dividing, but insertion sort goes straight to the conquest)
* Very fast for arrays that are almost sorted

**Shell’s Sort (extra slides).** Not covered on exam. Idea is to compare items that are a distance h apart from each other, starting from large h and reducing down to h=1. The last step where h=1 ensures that the array is sorted (since h=1 is just insertion sort). The earlier steps help speed things up by making long distance moves, fixing many inversions at once. Theoretical analysis of Shell’s sort is highly technical and has surprising results.

### **Mergesort**

We can sort by merging, as discussed in an earlier lecture. <mark style="color:red;">Mergesort is</mark> Θ<mark style="color:red;">(N log N) and uses</mark> Θ<mark style="color:red;">(N) memory.</mark>

* Split items into 2 roughly even pieces.
* Mergesort each half (steps not shown, this is a recursive algorithm!)
* Merge the two sorted halves to form the final result.
  * Compare input\[i] < input\[j] (if necessary).
  * Copy smaller item and increment p and i or j.
* Mergesort: [Demo](https://docs.google.com/presentation/d/1h-gS13kKWSKd\_5gt2FPXLYigFY4jf5rBkNFl3qZzRRw/edit?usp=sharing)

Time complexity, [analysis from previous lecture](https://docs.google.com/presentation/d/1\_LhI5V5JlcRHYU55\_SF7ZHxPemBr9OVlNzj7ScYdg64/edit#slide=id.g84271d11b\_2\_77): Θ(N log N runtime)

Space complexity with aux array: Costs Θ(N) memory.

### Partition Sort <a href="#overview" id="overview"></a>

**Partitioning.** Partioning an array on a <mark style="color:red;">pivot</mark> means to rearrange the array such that all items to the left of the pivot are $\leq$ the pivot, and all items to the right are $\geq$ the pivot. Naturally, the pivot can move during this process.

**Partitioning Strategies.** There are many particular strategies for partitioning. You are not expected to know any particular startegy.

**Quicksort**

* Partition on some pivot.&#x20;
* Quicksort to the left of the pivot.&#x20;
* Quicksort to the right.
* [Demo](https://docs.google.com/presentation/d/1QjAs-zx1i0\_XWlLqsKtexb-iueao9jNLkN-gW9QxAD0/edit?usp=sharing).

**Quicksort Runtime.** Understand how to show that in the best case, Quicksort has runtime Θ(N \log N), and in the worse case has runtime Θ(N^2).

**Pivot Selection.** Choice of partitioning strategy and <mark style="color:red;">pivot have profound impacts on runtime</mark>. **Two pivot selection strategies** that we discussed: Use leftmost item and pick a random pivot. Understand how using leftmost item can lead to bad performance on real data.

**Randomization.** Accept (without proof) that Quicksort has on average Θ(N \log N) runtime. Picking a random pivot or shuffling an array before sorting (using an appropriate partitioning strategy) ensures that we’re in the average case.

**Quicksort properties.** For most real world situations, quicksort is the fastest sort.

**Hoare Partitioning.** <mark style="color:red;">One very fast in-place technique for partitioning is to use a pair of pointers that start at the left and right edges of the array and move towards each other</mark>.&#x20;

<figure><img src=".gitbook/assets/image (132).png" alt="" width="375"><figcaption></figcaption></figure>

Create L and G pointers at left and right ends.

* L pointer is a friend to small items, and hates large or equal items.
* G pointer is a friend to large items, and hates small or equal items.
* Walk pointers towards each other, stopping on a hated item.
  * When both pointers have stopped, swap and move pointers by one.
  * When pointers cross, you are done walking.
* Swap pivot with G.
* Full details here: [Demo](https://docs.google.com/presentation/d/1DOnWS59PJOa-LaBfttPRseIpwLGefZkn450TMSSUiQY/pub?start=false\&loop=false\&delayms=3000)

<figure><img src=".gitbook/assets/image (133).png" alt="" width="375"><figcaption><p>When pointers cross, you are done walking</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (134).png" alt="" width="375"><figcaption><p>Swap pivot with G</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (135).png" alt="" width="375"><figcaption><p>Swap pivot with G</p></figcaption></figure>

### **Selection** <mark style="color:red;">Kth largest item</mark>

One way to solve this problem is with sorting, but we can do better.&#x20;

A linear time approach was developed in 1972 called PICK, but we did not cover this approach in class, because it is <mark style="color:red;">not as fast as the Quick Select technique</mark>.

**Quick Select.** <mark style="color:red;">Using partitioning, we can solve the selection problem in expected linear time</mark>. The algorithm is to:

* Simply partition the array,
* Quick select on the side of the array containing the median.&#x20;

Best case time is Θ(N), expected time is Θ(N), and worst case time is Θ(N^2). You should know how to show the best and worst case times. This algorithm is <mark style="color:red;">**the fastest known algorithm for finding the median**</mark>.

<figure><img src=".gitbook/assets/image (137).png" alt="" width="375"><figcaption><p>performance</p></figcaption></figure>



**Stability.** <mark style="color:red;">A sort is stable if the order of equal items is preserved</mark>. （如果从sorted item里面提取出来相等的Items，相等Items也是sorted，那么这个Sorting就是stable的）This is desirable, for example, if we want to sort on two different properties of our objects. Know how to show the stability or instability of an algorithm.

|                | Memory                | # Compares          | Notes                 | Stable? |
| -------------- | --------------------- | ------------------- | --------------------- | ------- |
| Heapsort       | Θ(1)                  | Θ(N log N)          | Bad caching (61C)     | No      |
| Insertion      | Θ(1)                  | Θ(N2)               | Θ(N) if almost sorted | Yes     |
| Mergesort      | Θ(N)                  | Θ(N log N)          |                       | Yes     |
| Quicksort LTHS | Θ(log N) (call stack) | Θ(N log N) expected | Fastest sort          | No      |

**Optimizing Sorts.** We can play a few tricks to speed up a sort.&#x20;

* One is to switch to insertion sort for small problems (< 15 items).&#x20;
* Another is to exploit existing order in the array. A sort that exploits existing order is sometimes called “adaptive”.&#x20;

Python and Java utilize a sort called **Timsort** that has a number of improvements, resulting in, for example Θ(N) performance on almost sorted arrays. A third trick, for worst case N^2 sorts only, is to make them switch to a worst case N \log N sort if they detect that they have exceeded a reasonable number of operations.

**Shuffling.** <mark style="color:red;">To shuffle an array, we can assign a random floating point number to every object, and sort based on those numbers.</mark> For information on generation of random numbers, see [Fall 2014 61B](https://www.google.com/url?q=https://docs.google.com/presentation/d/1uXMsukvTUI0m5\_6QfaYDmPDPXBXGRix7juEd7ekBjG0/pub?start%3Dfalse%26loop%3Dfalse%26delayms%3D3000%26slide%3Did.g46b429e30\_0110\&sa=D\&ust=1461443429774000\&usg=AFQjCNEiWI0CUmG1lyK8ZDIU6dY272cbdQ).

**Choose Sort Algorithm**

1，Stable: Insertingsort and Mergesort;

2, Only Speed: Quicksort (partition sort, 2-way quick sort)

3, For an array almost perfectly sorted: Insertingsort;
