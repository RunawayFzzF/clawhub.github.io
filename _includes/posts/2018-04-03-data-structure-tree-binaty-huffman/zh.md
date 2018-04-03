# 树、二叉树、哈夫曼树

## 树

> 树是n（n≥0）个结点的有限集。n=0时称为**空树**，在任意一棵**非空树**中：
>
> （1）有且仅有一个特定的称为**根（root）**的结点；
>
> （2）当n＞1时，其余结点可分为m（m>0）个互不相交的有限集T1、T2、T3......、Tm，其中每个集合本身又是一棵树，并且称为根的子树。

![](/img/in-post/2018-04-03-data-structure-tree-binaty-huffman/tree.jpg)

## 二叉树

> 二叉树的每个结点至多只有二棵子树(不存在度大于2的结点)，二叉树的子树有左右之分，次序不能颠倒。二叉树的第i层至多有2i-1个结点；深度为k的二叉树至多有2k-1个结点；对任何一棵二叉树T，如果其终端结点数为n0，度为2的结点数为n2，则n0=n2+1。

![](/img/in-post/2018-04-03-data-structure-tree-binaty-huffman/binary-tree.jpg)

## 哈夫曼树

> 哈夫曼树又称最优二叉树。它是 n 个带权叶子结点构成的所有二叉树中，带权路径长度 WPL 最小的二叉树。

![](/img/in-post/2018-04-03-data-structure-tree-binaty-huffman/huffman-tree.png)

## 参考资料

[哈夫曼树的基本构建与操作](https://blog.csdn.net/Move_now/article/details/53398753){:target="_blank"}

[关于数据结构中的树，这里有你想知道的一切](https://zhuanlan.zhihu.com/p/30918614){:target="_blank"}

[数据结构中常用的树](https://blog.csdn.net/hero_myself/article/details/52080969){:target="_blank"}

[Data Structure\] 数据结构中各种树{:target="_blank"}](http://www.cnblogs.com/maybe2030/p/4732377.html)

 [重温数据结构：树 及 Java 实现](https://blog.csdn.net/u011240877/article/details/53193877){:target="_blank"}

[从零开始_学_数据结构（二）——树的基本概念](https://blog.csdn.net/qq20004604/article/details/50936940){:target="_blank"}


