---
description: >-
  左偏红黑树是红黑树的一种变体，它的对红边（点）的位置做了一定限制，使得其插入与删除操作可以与 2-3 树构成一一对应。
  我们假设读者已经至少掌握了一种基于旋转的平衡树，因此本文不会对旋转操作进行讲解。
---

# 左偏红黑树

### 红黑树

#### 性质

一棵红黑树满足如下性质：

1. 节点是红色或黑色；
2. NIL 节点（空叶子节点）为黑色；
3. 红色的节点的所有儿子的颜色必须是黑色，即从每个叶子到根的所有路径上不能有两个连续的红色节点；
4. 从任一节点到其子树中的每个叶子的所有简单路径上都包含相同数目的黑色节点。（黑高平衡）

这保证了从根节点到任意叶子的最长路径（红黑交替）不会超过最短路径（全黑）的二倍。从而保证了树的平衡性。

维护这些性质是比较复杂的，如果我们要插入一个节点，首先，它一定会被染色成红色，否则会破坏性质 4。即使这样，我们还是有可能会破坏性质 3。因此需要进行调整。而删除节点就更加麻烦，与插入类似，我们不能删除黑色节点，否则会破坏黑高的平衡。如何方便地解决这些问题呢？

### 左偏红黑树（Left Leaning Red Black Tree）

#### 解释

左偏红黑树是一种容易实现的红黑树变体。

在以下左偏红黑树示意图中，是边具有颜色而不是节点具有颜色。我们习惯用一个节点的颜色代指它的父亲边的颜色。

左偏红黑树对红黑树进行了进一步限制，一个黑色节点的左右儿子：

* 要么全是黑色；
* 要么左儿子是红色，右儿子是黑色。

符合条件的情况：

![llrbt1](https://oi-wiki.org/ds/images/llrbt-1.png)

不符合条件的情况：

![llrbt2](https://oi-wiki.org/ds/images/llrbt-2.png)

这是左偏树的「左偏」性质：红色边只能是左偏的。

<details>

<summary>参考代码（部分）</summary>

<pre class="language-java"><code class="lang-java">// Some code
Node rotateLeft(Node h) {
    x = h.right;
    h.right = x.left;
    x.left = h;
    x.color = h.color;
    h.color = RED;
    return x;
}

Node rotateRight(Node h)
{
<strong>    x = h.left;
</strong>    h.left= x.right;
    x.right= h;
    x.color = h.color;
    h.color = RED;
    return x;
}

void flipColors(Node h)
{
    h.color = !h.color;
    h.left.color = !h.left.color;
    h.right.color = !h.right.color;
}
</code></pre>

</details>

#### 过程

**插入**

<mark style="color:red;">Always inserted as leaves.</mark>

我们首先使用普通的 BST 插入方法，<mark style="color:red;">在树的底部插入一个红色的叶子节点，然后通过从下向上的调整，使得插入后的树仍然符合左偏红黑树的性质</mark>。下面描述调整的过程：

![llrbt3](https://oi-wiki.org/ds/images/llrbt-3.png)

插入后，可能会产生一条右偏的红色边，因此需要对红边右偏的情况进行一次左旋：

![llrbt4](https://oi-wiki.org/ds/images/llrbt-4.png)

考虑左旋后会产生两条连续的左偏红色边：

![llrbt5](https://oi-wiki.org/ds/images/llrbt-5.png)

因此需要把它进行一次右旋。而对于右旋后的情况，我们应该对它进行 `color_flip`：即翻转该节点和它的两个儿子的颜色

![llrbt6](https://oi-wiki.org/ds/images/llrbt-6.png)

从而消灭右偏的红边。

For 2-3-4 tree: we flip colors to split any 4-node encountered <mark style="color:red;">on the way down</mark> the tree and do rotations to balance 4-nodes (eliminate occurrences of consecutive red links <mark style="color:red;">on the way up</mark> the tree)

<details>

<summary>参考代码（部分）</summary>

<pre class="language-java"><code class="lang-java">// Some code
public class LLRB&#x3C;Key extends Comparable&#x3C;Key>, Value>
{
<strong>    private static final boolean RED = true;
</strong>    private static final boolean BLACK = false;
    private Node root;
    private class Node {
        private Key key;
        private Value val;
        private Node left, right;
        private boolean color;
        Node(Key key, Value val) {
            this.key = key;
            this.val = val;
            this.color = RED;
        }
    }
    public Value search(Key key) {
<strong>        Node x = root;
</strong>        while (x != null) {
            int cmp = key.compareTo(x.key);
            if (cmp == 0) return x.val;
            else if (cmp &#x3C; 0) x = x.left;
            else if (cmp > 0) x = x.right;
        }
        return null;
    }
    
    public void insert(Key key, Value value) {
        root = insert(root, key, value);
<strong>        root.color = BLACK;
</strong>    }
    
    private Node insert(Node h, Key key, Value value) {
        
        if (h == null)
            return new Node(key, value);

        int cmp = key.compareTo(h.key);
        if (cmp == 0)
            h.val = value;
        else if (cmp &#x3C; 0)
            h.left = insert(h.left, key, value);
        else
            h.right = insert(h.right, key, value);
        
        // fix up
        if (isRed(h.right) &#x26;&#x26; !isRed(h.left))
            h = rotateLeft(h);
<strong>        if (isRed(h.left) &#x26;&#x26; isRed(h.left.left))
</strong><strong>            h = rotateRight(h);
</strong>        if (isRed(h.left) &#x26;&#x26; isRed(h.right))
            colorFlip(h);
        return h;
    }
}
</code></pre>

</details>

**删除**

<mark style="color:red;">删除操作基于这样的思想：我们不能删除黑色的节点，因为这样会破坏黑高。所以我们需要保证我们最后删除的节点是红色的。</mark>

**删除最小值节点**

首先来试一下删除整棵树里的最小值。

<mark style="color:red;">怎么才能保证最后删除的节点是红色的呢？我们需要在向下递归的过程中保证一个性质：如果当前节点是</mark> <mark style="color:red;"></mark><mark style="color:red;">`h`</mark><mark style="color:red;">，那么需要保证</mark> <mark style="color:red;"></mark><mark style="color:red;">`h`</mark> <mark style="color:red;"></mark><mark style="color:red;">是红色，或者</mark> <mark style="color:red;"></mark><mark style="color:red;">`h->lc`</mark> <mark style="color:red;"></mark><mark style="color:red;">是红色。</mark>

考虑这样做的正确性，如果我们能够通过各种旋转和反转颜色操作成功维护这个性质，那么当我们到达最小的节点 `h_min` 的时候，有 `h_min` 是红色，或者 `h_min` 的左子树——但是 `h_min` 根本没有左子树！所以这就保证了最小值节点一定是红的，既然它是红色的，我们就可以大胆的删除它，然后用与插入操作相同的调整思路对树进行调整。

下面我们来考虑怎么满足这个性质，注意，我们会在向下递归的时候 **临时地** 破坏左偏红黑树的若干性质，但是当我们从递归中返回时还会将其恢复。

如下图所描述的，是一种较为简单的情况，此时 `h->rc->lc` 为黑色，我们只需要一次翻转颜色即可：

![llrbt-7](https://oi-wiki.org/ds/images/llrbt-7.png)

并且，在如上所示的翻转之后，不会使 `h->rc` 与 `h->rc->lc` 形成连续的红边；

但如果 `h->rc->lc` 是红色，情况会比较复杂：

![llrbt-8](https://oi-wiki.org/ds/images/llrbt-8.png)

如果只进行翻转颜色，会产生连续的红边，而考虑我们递归返回的时候，是无法修复这样的情况的，因此需要进行处理。

然后就可以进行删除了：

<details>

<summary>参考代码（部分）</summary>

```java
// Some code
public void deleteMin() {
    root = deleteMin(root);
    root.color = BLACK;
}
private Node deleteMin(Node h) {

    if (h.left == null) return null; // this line is deleting node to null.

    // 只有当h->lc和h->lc->lc都是黑色的时候，才需要做旋转来保证可以继续找最小值；不用旋
    // 转，说明h->lc或h->lc-lc是红色的。
    // 原则：我们需要在向下递归的过程中保证一个性质：如果当前节点是 h，那么需要保
    //      证 h 是红色，或者 h->lc 是红色;
    // Easy Case保证了h->lc是红色；Harder Case 保证了h->lc->lc是红色。
    if (!isRed(h.left) && !isRed(h.left.left))
        h = moveRedLeft(h); // 这里对应的是 Easy Case 和 Harder Case两个处理。

    // 向左递归，这句之前是way down，这句之后是way up
    h.left = deleteMin(h.left);

    //
    return fixUp(h);
}
```

</details>

With fixUp(), we can leave rightleaning red links and unbalanced 4-nodes along the search path, secure that these conditions will be fixed on the way up the tree.

<details>

<summary>参考代码（部分）</summary>

```java
// Some code
public Node fixUp(Node h) {
    if (isRed(h.right) && !isRed(h.left))
        h = rotateLeft(h);
    if (isRed(h.left) && isRed(h.left.left))
        h = rotateRight(h);
    if (isRed(h.left) && isRed(h.right))
        colorFlip(h);
    return h;
}

```

</details>

**删除任意节点**

我们首先考虑删除叶子：与删最小值类似，我们在删除任意值的过程中也要维护一个性质，不过这次比较特殊，因为我们不是只向左边走，而是可以向左右两个方向走，因此在删除过程中维护的性质是这样的：<mark style="color:red;">如果往左走，当前节点是</mark> <mark style="color:red;"></mark><mark style="color:red;">`h`</mark><mark style="color:red;">，那么需要保证</mark> <mark style="color:red;"></mark><mark style="color:red;">`h`</mark> <mark style="color:red;"></mark><mark style="color:red;">是红色，或者</mark> <mark style="color:red;"></mark><mark style="color:red;">`h->lc`</mark> <mark style="color:red;"></mark><mark style="color:red;">是红色；如果往右走，当前节点是</mark> <mark style="color:red;"></mark><mark style="color:red;">`h`</mark><mark style="color:red;">，那么需要保证</mark> <mark style="color:red;"></mark><mark style="color:red;">`h`</mark> <mark style="color:red;"></mark><mark style="color:red;">是红色，或者</mark> <mark style="color:red;"></mark><mark style="color:red;">`h->rc`</mark> <mark style="color:red;"></mark><mark style="color:red;">是红色。这样可以保证我们最后总会删掉一个红色节点。</mark>

下面考虑删除非叶子节点，我们只需要找到其右子树（如果有）里的最小节点，然后用右子树的最小节点的值代替该节点的值，最后删除右子树里的最小节点。

![llrbt-9](https://oi-wiki.org/ds/images/llrbt-9.png)

那如果没有右子树怎么办？我们需要把左子树旋转过来，这样就不会出现这个问题了。

<details>

<summary>参考代码</summary>

<pre class="language-java"><code class="lang-java">// Some code
private Node moveRedLeft(Node h) {
    colorFlip(h);
    if (isRed(h.right.left)) {
        h.right = rotateRight(h.right);
        h = rotateLeft(h);
        colorFlip(h);
    }
    return h;
}

private Node moveRedRight(Node h) {
    colorFlip(h);
    if (isRed(h.left.left)) {
        h = rotateRight(h);
        colorFlip(h);
    }
    return h;
}

public void delete(Key key) {
    root = delete(root, key);
    root.color = BLACK;
}
private Node delete(Node h, Key key) {
    if (key.compareTo(h.key) &#x3C; 0) {
        
        // 只有当h->lc和h->lc->lc都是黑色的时候，才需要做旋转来保证可以继续找最小值；不用旋
        // 转，说明h->lc或h->lc-lc是红色的。
        // 原则：我们需要在向下递归的过程中保证一个性质：如果当前节点是 h，那么需要保
        //      证 h 是红色，或者 h->lc 是红色;
        // Easy Case保证了h->lc是红色；Harder Case 保证了h->lc->lc是红色。
<strong>        if (!isRed(h.left) &#x26;&#x26; !isRed(h.left.left))
</strong>            h = moveRedLeft(h);
        h.left = delete(h.left, key);
    } else {
    
        // 因为是往右走，要保证h-rc是红色的，既然左边是红色的，那就rotateRight
        if (isRed(h.left))
            h = rotateRight(h);
        
        // 这个是叶子的Delete操作
        if (key.compareTo(h.key) == 0 &#x26;&#x26; (h.right == null))
            return null;
        
        // 只有当h->rc和h->rc->rc都是黑色的时候，才需要做旋转来保证可以继续找最小值；不用旋
        // 转，说明h->rc或h->rc-rc是红色的。
        // 原则：我们需要在向下递归的过程中保证一个性质：如果当前节点是 h，那么需要保
        //      证 h 是红色，或者 h->rc 是红色;
        // Easy Case保证了h->rc是红色；Harder Case 保证了h->rc->rc是红色。
        if (!isRed(h.right) &#x26;&#x26; !isRed(h.right.left))
            h = moveRedRight(h);
        
        // 这个是非叶子的Delete操作
        // (1)找到右子树（如果有）里的最小节点，
        // (2)用右子树的最小节点的值代替该节点的值，
        // (3)删除右子树里的最小节点。
        if (key.compareTo(h.key) == 0) {
<strong>            h.val = get(h.right, min(h.right).key);
</strong>            h.key = min(h.right).key;
            h.right = deleteMin(h.right);
        }
        else
            h.right = delete(h.right, key);
    }
    return fixUp(h);
}
</code></pre>

</details>

### 实现

下面的代码是用左偏红黑树实现的 `Set`，即有序不可重集合：

<details>

<summary>参考代码</summary>



</details>

### 参考资料与拓展阅读

* [Left-Leaning Red-Black Trees](https://sedgewick.io/wp-content/themes/sedgewick/papers/2008LLRB.pdf)- Robert Sedgewick Princeton University
* [Balanced Search Trees](https://algs4.cs.princeton.edu/lectures/keynote/33BalancedSearchTrees-2x2.pdf)-\_Algorithms\_Robert Sedgewick | Kevin Wayne
