# 最小生成树

## 相关定义



> - **连通图**：在无向图中，若任意两个顶点vivi与vjvj都有路径相通，则称该无向图为连通图。
> - **强连通图**：在有向图中，若任意两个顶点vivi与vjvj都有路径相通，则称该有向图为强连通图。
> - **连通网**：在连通图中，若图的边具有一定的意义，每一条边都对应着一个数，称为权；权代表着连接连个顶点的代价，称这种连通图叫做连通网。
> - **生成树**：一个连通图的生成树是指一个连通子图，它含有图中全部n个顶点，但只有足以构成一棵树的n-1条边。一颗有n个顶点的生成树有且仅有n-1条边，如果生成树中再添加一条边，则必定成环。
> - **最小生成树**：在连通网的所有生成树中，所有边的代价和最小的生成树，称为最小生成树。 

![](/img/in-post/2018-04-11-data-structure-minimum-spanning-tree/minimum-spanning-tree.jpg)

## 算法

##### 普里姆（Prim）算法

> 取图中任意一个顶点V作为生成树的根，之后若要往生成树上添加顶点W，则在顶点V和W之间必定存在一条边。并且该边的权值在所有连通顶点V和W之间的边中取值最小。

![](/img/in-post/2018-04-11-data-structure-minimum-spanning-tree/prim.jpg)

##### 克鲁斯克尔（Kruskal）算法

> 有n个顶点的最小生成树含有n-1条边。 
> 按权值递增次序选择合适的边来构造最小生成树。先把所有的顶点包括在生成树中，将图中的边按权值递增的顺序依次选取，要保证选取的边不使生成树中产生回路，将选取的边加到生成树中，直至有n-1条边为止。

![](/img/in-post/2018-04-11-data-structure-minimum-spanning-tree/kruskal.jpg)

## 参考资料

[图的最小生成树：Prim算法和Kruskal算法](https://blog.csdn.net/jinzhao1993/article/details/51278707){:target="_blank"}

[算法导论--最小生成树（Kruskal和Prim算法）](https://blog.csdn.net/luoshixian099/article/details/51908175){:target="_blank"}

[数据结构--最小生成树详解](https://blog.csdn.net/qq_35644234/article/details/59106779){:target="_blank"}

[