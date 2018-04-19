### 拓扑排序

#### 相关概念

> 对有向无环图（DAG, Directed Acyclic Graph）G进行拓扑排序，是将G中所有顶点排成一个线性序列，使得图中任意一对顶点u和v，若边(u,v)∈E(G)，则u在线性序列中出现在v之前。 
>
> `AOV网`(Activity On Vertex network)：顶点表示活动、边表示活动间先后关系的有向图称做顶点活动网。
>
> 在AOV网中，若不存在回路，则所有活动可排列成一个线性序列，使得每个活动的所有前驱活动都排在该活动的前面，我们把此序列叫做`拓扑序列`(Topological order)。
>
> 由AOV网构造拓扑序列的过程叫做`拓扑排序`(Topological sort)。

#### 实现步骤

1. 在有向图中选一个没有前驱的顶点并且输出
2. 从图中删除该顶点和所有以它为尾的弧（白话就是：删除所有和它有关的边）
3. 重复上述两步，直至所有顶点输出，或者当前图中不存在无前驱的顶点为止，后者代表我们的有向图是有环的，因此，也可以通过拓扑排序来判断一个图是否有环。

##### 应用

拓扑排序通常用来“排序”具有依赖关系的任务。

比如，如果用一个DAG图来表示一个工程，其中每个顶点表示工程中的一个任务，用有向边<A,B><A,B>表示在做任务 B 之前必须先完成任务 A。故在这个工程中，任意两个任务要么具有确定的先后关系，要么是没有关系，绝对不存在互相矛盾的关系（即环路）。

#### 案例一

如果我们有如下的一个有向无环图，我们需要对这个图的顶点进行拓扑排序，过程如下： 
![这里写图片描述](https://img-blog.csdn.net/20170306153942850?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

首先，我们发现V6和v1是没有前驱的，所以我们就随机选去一个输出，我们先输出V6，删除和V6有关的边，得到如下图结果： 
![这里写图片描述](https://img-blog.csdn.net/20170306154112495?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后，我们继续寻找没有前驱的顶点，发现V1没有前驱，所以输出V1，删除和V1有关的边，得到下图的结果： 
![这里写图片描述](https://img-blog.csdn.net/20170306154245339?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后，我们又发现V4和V3都是没有前驱的，那么我们就随机选取一个顶点输出（具体看你实现的算法和图存储结构），我们输出V4，得到如下图结果： 
![这里写图片描述](https://img-blog.csdn.net/20170306154634843?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后，我们输出没有前驱的顶点V3，得到如下结果： 
![这里写图片描述](https://img-blog.csdn.net/20170306154712704?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后，我们分别输出V5和V2，最后全部顶点输出完成，该图的一个拓扑序列为：

v6–>v1—->v4—>v3—>v5—>v2



##### 案例二

例如，下面这个图：

![img](https://img-blog.csdn.net/20150507001028284) 

它是一个 DAG 图，那么如何写出它的拓扑排序呢？这里说一种比较常用的方法：

1. 从 DAG 图中选择一个 没有前驱（即入度为0）的顶点并输出。
2. 从图中删除该顶点和所有以它为起点的有向边。
3. 重复 1 和 2 直到当前的 DAG 图为空或**当前图中不存在无前驱的顶点为止**。后一种情况说明有向图中必然存在环。

![img](https://img-blog.csdn.net/20150507001759702) 

于是，得到拓扑排序后的结果是 { 1, 2, 4, 3, 5 }。

通常，一个有向无环图可以有**一个或多个**拓扑排序序列。

