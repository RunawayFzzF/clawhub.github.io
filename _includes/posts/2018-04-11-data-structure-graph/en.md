# 图

## 定义



> 图（Graph）是由顶点的有穷非空集合和顶点之间边的集合组成，通常表示为：G（V，E），其中，G表示一个图，V是图G中顶点的集合，E是图G中边的集合。

![](/img/in-post/2018-04-11-data-structure-graph/graph.png)

## 图的存储结构

#### 邻接矩阵

> 邻接矩阵存储使用2个数组存储图的信息：1个以为数组存储顶点，一个二维数组存储边的信息
> （1）二维数组中的对角线为0，以为不存在顶点到自身的边
> （2）要知道某个点的出度，就是顶点vi在第i行的元素之和，入度就是该顶点所在列的元素之和
> （3）顶点vi的所有邻接点就是把矩阵中第i行元素扫描一遍

![](/img/in-post/2018-04-11-data-structure-graph/neighborhood_rectangle.png)

#### 邻接表

> 邻接表：数组 + 链表
> （1）用的数组存储每个节点
> （2）数组中的每个节点的所有邻接点组成一个链表(因为邻接点的个数不确定)。

![](/img/in-post/2018-04-11-data-structure-graph/neighbor_table.png)

## 参考资料

[数据结构之图](https://blog.csdn.net/z4909801/article/details/77923582){:target="_blank"}

[数据结构与算法（六），图](https://www.jianshu.com/p/a9f8b69813d5){:target="_blank"}

[数据结构 - 图的基本术语](https://blog.csdn.net/wangzi11322/article/details/45417081){:target="_blank"}

[浅析数据结构-图的基本概念](http://www.cnblogs.com/polly333/p/4760275.html){:target="_blank"}

[数据结构（七）图](http://www.cnblogs.com/moonlord/p/5938061.html){:target="_blank"}