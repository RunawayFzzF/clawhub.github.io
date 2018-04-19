### LevelDB内部存储模型 ###
####运行一段时间后levelDB的文件####

![](/img/in-post/2018-04-19-level-db-storage-structure/leveldb-store.png)

#### 存储模型

![](/img/in-post/2018-04-19-level-db-storage-structure/leveldb-structure.png)

#### 简要介绍各个文件含义

内存中的MemTable和Immutable MemTable，磁盘中的log和SSTable文件是用来存储K-V记录的。

Manifest 就记载了SSTable各个文件的管理信息，比如属于哪个Level，文件名称，最小key和最大key各自是多少。下图是Manifest所存储内容的示意：

![](/img/in-post/2018-04-19-level-db-storage-structure/leveldb-manifest.png)

另外，在LevleDb的运行过程中，随着Compaction的进行，SSTable文件会发生变化，会有新的文件产生，老的文件被废弃，Manifest也会跟着反映这种变化，此时往往会新生成Manifest文件来记载这种变化，而Current则用来指出哪个Manifest文件才是我们关心的那个Manifest文件。