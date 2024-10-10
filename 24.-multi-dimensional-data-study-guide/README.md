# 24. Multi-Dimensional Data Study Guide

Author: Kartik Kapur

### QA [#](broken-reference) <a href="#qa" id="qa"></a>

Linked [here](https://youtu.be/NAz4u\_DdSqI).

### Check-in Exercise [#](broken-reference) <a href="#check-in-exercise" id="check-in-exercise"></a>

Linked [here](https://forms.gle/HgtAGmNKscfecc2v7).

### Overview [#](broken-reference) <a href="#overview" id="overview"></a>

**Additional Set Operations** There are many other operations we might be interested in supporting on a set.&#x20;

* `select(int i)` method that returns the ith smallest item in the set.&#x20;
* `subSet(T from, T to)` operation that returns all items in the set between `from` and `to`.&#x20;
* `nearest(T x)` method that finds the closest item in the set to x.
* `rank(T x)`: Returns the “rank” of x in the set (opposite of select).

On 1D data, it is relatively straightforward to support such operations efficiently. If we use only one of the coordinates (e.g. X or Y coordinate), the structure of our data will fail to reflect the full ordering of the data.

**QuadTrees**

A natural approach is to make a new type of Tree– the QuadTree. <mark style="color:red;">The QuadTree has 4 neighbors, Northwest,Northeast, Southwest, and Southeast.</mark> As you move your way down the tree to support queries, it is possible to prune branches that do not contain a useful result.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

**K-D Trees** One final data structure that we have for dealing with 2 dimensional data is the K-d Tree. <mark style="color:red;">Essentially the idea of a K-D tree is that it’s a normal Binary Search Tree, except we alternate what value we’re looking at when we traverse through the tree.</mark> For example at the root everything to the left has an X value less than the root and everything to the right has a X value greater than the root. Then on the next level, every item to the left of some node has a Y value less than that item and everything to the right has a Y value greater than it. Somewhat surprisingly, KdTrees are quite efficient.

[K-d tree nearest demo.](https://docs.google.com/presentation/d/1DNunK22t-4OU\_9c-OBgKkMAdly9aZQkWuv\_tBkDg1G4/edit?usp=sharing)

k-d tree example for 2-d:

* Basic idea, root node partitions entire space into left and right (by x).
* All depth 1 nodes partition subspace into up and down (by y).
* All depth 2 nodes partition subspace into left and right (by x).

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

**Nearest Example**

In case you’re curious, the key change is that now we think of each point as having a “**good side**” and a “**bad side**”.

* <mark style="color:red;">You always explore the good side</mark>
* <mark style="color:red;">You</mark> <mark style="color:red;"></mark><mark style="color:red;">**only explore the bad side**</mark> <mark style="color:red;"></mark><mark style="color:red;">if there’s a chance that it could contain something better.</mark>

Project note: It is incredibly important that you <mark style="color:red;">consider the correct child first</mark>! Your code will be much slower if you always use the “left” link before the “right” one. <mark style="color:red;">Intuition is that we want to search more promising parts of the space first, so we can prune less promising parts later.</mark>

#### **How to judge if there could be something better at the bad side or not?**

* Should consider the best distance possible in the bad area, for the picture below is the length of the  marked line dotted line, which is smaller than the recorded best distance Distance((0, 7), E(1, 5)).
* Another way to see this is draw a hyperspere with Target (0, 7) as center and  Distance((0, 7), E(1, 5)) as radius, if it is intersecting with the bad side, then the answer is YES.

<figure><img src="../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

Project note: You can simplify your code by only measuring the length of the green dashed vertical line rather than the purple diagonal hypotenuse. So here, we’d compute goal.y - d.y (which is pretty easy!)

* Warning, the Point class in proj 2b actually uses the squared distance, so you’ll need to compare with (goal.y - d.y)^2.
* Or even better, you can create the best hypothetical point and use Point.distance.

Effectively <mark style="color:red;">the green pruning rule is less aggressive than the purple one, so we might sometimes look at a “bad side” that has no possible</mark> better points. However, the resulting answer will still be correct.

<figure><img src="../.gitbook/assets/image (102).png" alt="" width="375"><figcaption></figcaption></figure>

#### **K-d Tree Nearest Pseudocode**

<mark style="color:red;">即使不做prune，最后计算得到的结果也会是正确的，只是速度会变慢，相当于访问了所有节点？</mark>

nearest(Node n, Point goal, Node best):

* If n is null, return best
* If n.distance(goal) < best.distance(goal), best = n
*   If goal < n (<mark style="color:red;">according to n’s comparator</mark>):

    * goodSide = n.”left”Child
    * badSide = n.”right”Child

    else:

    * goodSide = n.”right”Child
    * badSide = n.”left”Child
* best = **nearest(goodSide, goal, best)**
* <mark style="color:red;">**If bad side could still have something useful (to prune or not?)**</mark>
  * <mark style="color:red;">best = nearest(badSide, goal, best)</mark>
* return best

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

#### Summary

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>



* [QA](broken-reference)
* [Check-in Exercise](broken-reference)
* [Overview](broken-reference)
