### JAVA CPU占用过高问题排查(linux)

#### 一 linux查看进程信息

```sh
top
```

![](/img/in-post/2018-04-20-java-cup-linux/1-top.png)

#### 二 查看进程占用cpu最多的线程

```sh
ps -mp 23967 -o THREAD,tid,time
```

![](/img/in-post/2018-04-20-java-cup-linux/2-查看进程中线程.png)

#### 三 线程ID转16进制

```sh
printf "%x\n" 23968
```

![](/img/in-post/2018-04-20-java-cup-linux/3-线程ID转16进制.png)

#### 四 查看线程信息

```sh
jstack  23967  |grep -A  10  5da0
```

![](/img/in-post/2018-04-20-java-cup-linux/4-查看线程信息.png)

```sh
jstack 23967  |grep 5da0 -A 30
```

![](/img/in-post/2018-04-20-java-cup-linux/5-查看线程信息2.png)

#### 五 查看进程的对象信息

```sh
jmap -histo:live 23967 | more
```

![](/img/in-post/2018-04-20-java-cup-linux/6-查看进程对象.png)

#### 六 查看进程的GC情况

```sh
jstat -gcutil 23967 1000 100
```

![](/img/in-post/2018-04-20-java-cup-linux/7-GC.png)
