## 图的遍历

### 深度优先遍历

> 深度优先遍历是利用了栈的原理来对图的顶点进行访问，我们总是通过当前顶点的第一条出边（专业术语是：出度）（边的结束顶点下标最小并且这条边的结束顶点还未被访问过）来访问这条出边的结束顶点，之后对当前顶点做同样的处理，直到所有的顶点都被访问完成。

![](/img/in-post/2018-04-12-data-structure-graph-search/directed_graph_depth_fisrst-search.png)

![](/img/in-post/2018-04-12-data-structure-graph-search/undirected_graph_depth_fisrst-search.png)

### 广度优先遍历

> 广度优先遍历算法思想是借助队列来完成的，我们每次对当前顶点的所有的出边（出度）进行讨论，如果某条边的结束顶点还未被访问过，那么就把顶点加入到队列的队尾中，直到当前顶点的所有边都讨论完了，我们再从队列头部取出一个顶点，继续对这个顶点的出边进行讨论，重复这个过程，直到队列为空。

![](/img/in-post/2018-04-12-data-structure-graph-search/directed_graph_breadth_first_earch.png)

![](/img/in-post/2018-04-12-data-structure-graph-search/undirected_graph_breadth_first_earch.png)

## 参考资料

[图的遍历](https://blog.csdn.net/Hacker_ZhiDian/article/details/61260543){:target="_blank"}_

[图的深度遍历和广度遍历](http://www.cnblogs.com/weizhixiang/p/5815994.html){:target="_blank"}