## LevelDB

### 简介

> LevelDB是Google开源的持久化KV单机数据库，具有很高的随机写，顺序读/写性能，但是随机读的性能很一般，也就是说，LevelDB很适合应用在查询较少，而写很多的场景。LevelDB应用了[LSM](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.44.2782&rep=rep1&type=pdf) (Log Structured Merge) 策略，lsm_tree对索引变更进行延迟及批量处理，并通过一种类似于归并排序的方式高效地将更新迁移到磁盘，降低索引插入开销，关于LSM，本文在后面也会简单提及。

![](/img/in-post/2018-04-12-level-db-basics/level_db.png)

### Java使用

> 原生leveldb是基于C++开发，java语言无法直接使用；iq80对leveldb使用JAVA语言进行了“逐句”重开发，经过很多大型项目的验证（比如ActiveMQ），iq80开发的JAVA版leveldb在性能上损失极少（10%）。对于JAVA开发人员来说，我们直接使用即可，无需额外的安装其他lib。

#### Maven依赖

```xml
<dependency>
   <groupId>org.iq80.leveldb</groupId>
   <artifactId>leveldb</artifactId>
   <version>0.10</version>
</dependency>

<dependency>
   <groupId>org.iq80.leveldb</groupId>
   <artifactId>leveldb-api</artifactId>
   <version>0.10</version>
</dependency>
```

#### 代码例子

```java
boolean cleanup = true;  
Charset charset = Charset.forName("utf-8");  
String path = "/data/leveldb";  
  
//init  
DBFactory factory = Iq80DBFactory.factory;  
File dir = new File(path);  
//如果数据不需要reload，则每次重启，尝试清理磁盘中path下的旧数据。  
if(cleanup) {  
    factory.destroy(dir,null);//清除文件夹内的所有文件。  
}  
Options options = new Options().createIfMissing(true);  
//重新open新的db  
DB db = factory.open(dir,options);  
  
//write  
db.put("key-01".getBytes(charset),"value-01".getBytes(charset));  
  
//write后立即进行磁盘同步写  
WriteOptions writeOptions = new WriteOptions().sync(true);//线程安全  
db.put("key-02".getBytes(charset),"value-02".getBytes(charset),writeOptions);  
  
  
//batch write；  
WriteBatch writeBatch = db.createWriteBatch();  
writeBatch.put("key-03".getBytes(charset),"value-03".getBytes(charset));  
writeBatch.put("key-04".getBytes(charset),"value-04".getBytes(charset));  
writeBatch.delete("key-01".getBytes(charset));  
db.write(writeBatch);  
writeBatch.close();  
  
//read  
byte[] bv = db.get("key-02".getBytes(charset));  
if(bv != null && bv.length > 0) {  
    String value = new String(bv,charset);  
    System.out.println(value);  
}  
  
//iterator，遍历，顺序读  
  
//读取当前snapshot，快照，读取期间数据的变更，不会反应出来  
Snapshot snapshot = db.getSnapshot();  
//读选项  
ReadOptions readOptions = new ReadOptions();  
readOptions.fillCache(false);//遍历中swap出来的数据，不应该保存在memtable中。  
readOptions.snapshot(snapshot);//默认snapshot为当前。  
DBIterator iterator = db.iterator(readOptions);  
while (iterator.hasNext()) {  
    Map.Entry<byte[],byte[]> item = iterator.next();  
    String key = new String(item.getKey(),charset);  
    String value = new String(item.getValue(),charset);//null,check.  
    System.out.println(key + ":" + value);  
}  
iterator.close();//must be  
  
//delete  
db.delete("key-01".getBytes(charset));  
  
//compaction，手动  
  
db.compactRange("key-".getBytes(charset),null);  
  
//  
db.close();  
```

## 参考资料

[数据分析与处理之二（Leveldb 实现原理）](http://www.cnblogs.com/haippy/archive/2011/12/04/2276064.html){:target="_blank"}

[LevelDB库简介](http://www.cnblogs.com/chenny7/p/4026447.html){:target="_blank"}

[leveldb原理和使用](https://blog.csdn.net/chary8088/article/details/54945303){:target="_blank"}

[leveldb入门](https://blog.csdn.net/u012734441/article/details/79797684){:target="_blank"}

[leveldb.org](http://leveldb.org/){:target="_blank"}