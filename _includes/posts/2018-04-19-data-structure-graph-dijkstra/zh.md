#### 单源最小路径

##### 定义

> 从某顶点出发，沿图的边到另一顶点所经过的路径中，各边上权值之和最小的一条路径。

##### Dijkstra算法:解决单源最短路径问题

> - 适用范围：无负权回路，边权必须非负，单源最短路 
> - 时间复杂度：优化前O(n2)

##### 案例分析一

如求下图中的1号顶点到2、3、4、5、6号顶点的最短路径。

![090644t797fce7n20of7j9.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090644t797fce7n20of7j9.png)

这里使用二维数组e来存储顶点之间边的关系，初始值如下。

![090651l6pt4666tptut66u.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090651l6pt4666tptut66u.png)

我们还需要用一个一维数组dis来存储1号顶点到其余各个顶点的初始路程，如下。

![090657ofidcactthcig33i.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090657ofidcactthcig33i.png)

我们将此时dis数组中的值称为最短路的“估计值”。

​       既然是求1号顶点到其余各个顶点的最短路程，那就先找一个离1号顶点最近的顶点。通过数组dis可知当前离1号顶点最近是2号顶点。当选择了2号顶点后，dis[2]的值就已经从“估计值”变为了“确定值”，即1号顶点到2号顶点的最短路程就是当前dis[2]值。为什么呢？你想啊，目前离1号顶点最近的是2号顶点，并且这个图所有的边都是正数，那么肯定不可能通过第三个顶点中转，使得1号顶点到2号顶点的路程进一步缩短了。因为1号顶点到其它顶点的路程肯定没有1号到2号顶点短，对吧O(∩_∩)O~

​       既然选了2号顶点，接下来再来看2号顶点有哪些出边呢。有2->3和2->4这两条边。先讨论通过2->3这条边能否让1号顶点到3号顶点的路程变短。也就是说现在来比较dis[3]和dis[2]+e[2][3]的大小。其中dis[3]表示1号顶点到3号顶点的路程。dis[2]+e[2][3]中dis[2]表示1号顶点到2号顶点的路程，e[2][3]表示2->3这条边。所以dis[2]+e[2][3]就表示从1号顶点先到2号顶点，再通过2->3这条边，到达3号顶点的路程。

​       我们发现dis[3]=12，dis[2]+e[2][3]=1+9=10，dis[3]>dis[2]+e[2][3]，因此dis[3]要更新为10。这个过程有个专业术语叫做“松弛”。即1号顶点到3号顶点的路程即dis[3]，通过2->3这条边松弛成功。这便是Dijkstra算法的主要思想：通过“边”来松弛1号顶点到其余各个顶点的路程。

​       同理通过2->4（e[2][4]），可以将dis[4]的值从∞松弛为4（dis[4]初始为∞，dis[2]+e[2][4]=1+3=4，dis[4]>dis[2]+e[2][4]，因此dis[4]要更新为4）。

​       刚才我们对2号顶点所有的出边进行了松弛。松弛完毕之后dis数组为：

![090706vmjy7l2ee2lyalia.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090706vmjy7l2ee2lyalia.png)

​       接下来，继续在剩下的3、4、5和6号顶点中，选出离1号顶点最近的顶点。通过上面更新过dis数组，当前离1号顶点最近是4号顶点。此时，dis[4]的值已经从“估计值”变为了“确定值”。下面继续对4号顶点的所有出边（4->3，4->5和4->6）用刚才的方法进行松弛。松弛完毕之后dis数组为：

![090714f2p1wppynngj2pep.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090714f2p1wppynngj2pep.png)

​       继续在剩下的3、5和6号顶点中，选出离1号顶点最近的顶点，这次选择3号顶点。此时，dis[3]的值已经从“估计值”变为了“确定值”。对3号顶点的所有出边（3->5）进行松弛。松弛完毕之后dis数组为：

![090722ywunackk35i8cni5.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090722ywunackk35i8cni5.png)

​       继续在剩下的5和6号顶点中，选出离1号顶点最近的顶点，这次选择5号顶点。此时，dis[5]的值已经从“估计值”变为了“确定值”。对5号顶点的所有出边（5->4）进行松弛。松弛完毕之后dis数组为：

![090730eq6oqzyq7laqha9y.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090730eq6oqzyq7laqha9y.png)

​       最后对6号顶点所有点出边进行松弛。因为这个例子中6号顶点没有出边，因此不用处理。到此，dis数组中所有的值都已经从“估计值”变为了“确定值”。

​       最终dis数组如下，这便是1号顶点到其余各个顶点的最短路径。

![090738azt5clcozl899ekt.png](http://bbs.ahalei.com/data/attachment/forum/201403/31/090738azt5clcozl899ekt.png)

​       OK，现在来总结一下刚才的算法。算法的基本思想是：每次找到离源点（上面例子的源点就是1号顶点）最近的一个顶点，然后以该顶点为中心进行扩展，最终得到源点到其余所有点的最短路径。基本步骤如下：

- 将所有的顶点分为两部分：已知最短路程的顶点集合P和未知最短路径的顶点集合Q。最开始，已知最短路径的顶点集合P中只有源点一个顶点。我们这里用一个book[ i ]数组来记录哪些点在集合P中。例如对于某个顶点i，如果book[ i ]为1则表示这个顶点在集合P中，如果book[ i ]为0则表示这个顶点在集合Q中。
- 设置源点s到自己的最短路径为0即dis=0。若存在源点有能直接到达的顶点i，则把dis[ i ]设为e[s][ i ]。同时把所有其它（源点不能直接到达的）顶点的最短路径为设为∞。
- 在集合Q的所有顶点中选择一个离源点s最近的顶点u（即dis[u]最小）加入到集合P。并考察所有以点u为起点的边，对每一条边进行松弛操作。例如存在一条从u到v的边，那么可以通过将边u->v添加到尾部来拓展一条从s到v的路径，这条路径的长度是dis[u]+e[u][v]。如果这个值比目前已知的dis[v]的值要小，我们可以用新值来替代当前dis[v]中的值。
- 重复第3步，如果集合Q为空，算法结束。最终dis数组中的值就是源点到所有顶点的最短路径。

##### 案例分析二

下面我求下图，从顶点v1到其他各个顶点的最短路径

![这里写图片描述](https://img-blog.csdn.net/20170308144724663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

首先第一步，我们先声明一个dis数组，该数组初始化的值为： 
![这里写图片描述](https://img-blog.csdn.net/20170308150247263?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

我们的顶点集T的初始化为：T={v1}

既然是求 v1顶点到其余各个顶点的最短路程，那就先找一个离 1 号顶点最近的顶点。通过数组 dis 可知当前离v1顶点最近是 v3顶点。当选择了 2 号顶点后，dis[2]（下标从0开始）的值就已经从“估计值”变为了“确定值”，即 v1顶点到 v3顶点的最短路程就是当前 dis[2]值。将V3加入到T中。 
**为什么呢？因为目前离 v1顶点最近的是 v3顶点，并且这个图所有的边都是正数，那么肯定不可能通过第三个顶点中转，使得 v1顶点到 v3顶点的路程进一步缩短了。因为 v1顶点到其它顶点的路程肯定没有 v1到 v3顶点短.**

OK，既然确定了一个顶点的最短路径，下面我们就要根据这个新入的顶点V3会有出度，发现以v3 为弧尾的有： < v3,v4 >,那么我们看看路径：v1–v3–v4的长度是否比v1–v4短，其实这个已经是很明显的了，因为dis[3]代表的就是v1–v4的长度为无穷大，而v1–v3–v4的长度为：10+50=60，所以更新dis[3]的值,得到如下结果： 
![这里写图片描述](https://img-blog.csdn.net/20170308150707766?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

因此 dis[3]要更新为 60。这个过程有个专业术语叫做“松弛”。即 v1顶点到 v4顶点的路程即 dis[3]，通过 < v3,v4> 这条边松弛成功。这便是 Dijkstra 算法的主要思想：通过“边”来松弛v1顶点到其余各个顶点的路程。

然后，我们又从除dis[2]和dis[0]外的其他值中寻找最小值，发现dis[4]的值最小，通过之前是解释的原理，可以知道v1到v5的最短距离就是dis[4]的值，然后，我们把v5加入到集合T中，然后，考虑v5的出度是否会影响我们的数组dis的值，v5有两条出度：< v5,v4>和 < v5,v6>,然后我们发现：v1–v5–v4的长度为：50，而dis[3]的值为60，所以我们要更新dis[3]的值.另外，v1-v5-v6的长度为：90，而dis[5]为100，所以我们需要更新dis[5]的值。更新后的dis数组如下图： 
![这里写图片描述](https://img-blog.csdn.net/20171205193212203?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后，继续从dis中选择未确定的顶点的值中选择一个最小的值，发现dis[3]的值是最小的，所以把v4加入到集合T中，此时集合T={v1,v3,v5,v4},然后，考虑v4的出度是否会影响我们的数组dis的值，v4有一条出度：< v4,v6>,然后我们发现：v1–v5–v4–v6的长度为：60，而dis[5]的值为90，所以我们要更新dis[5]的值，更新后的dis数组如下图： 
![这里写图片描述](https://img-blog.csdn.net/20170308151732132?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后，我们使用同样原理，分别确定了v6和v2的最短路径，最后dis的数组的值如下： 
![这里写图片描述](https://img-blog.csdn.net/20170308152038851?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzU2NDQyMzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

因此，从图中，我们可以发现v1-v2的值为：∞，代表没有路径从v1到达v2。所以我们得到的最后的结果为：

```
起点  终点    最短路径    长度
v1    v2     无          ∞    
      v3     {v1,v3}    10
      v4     {v1,v5,v4}  50
      v5     {v1,v5}    30
      v6     {v1，v5,v4,v6} 60
```