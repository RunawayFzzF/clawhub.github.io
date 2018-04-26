### MongoDB的迭代器的用法

#### 版本

SpringBoot 2.0.1.RELEASE
JDK 1.8

#### MongoDB依赖

```xml
   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-mongodb</artifactId>
   </dependency>
```

#### 核心代码

```java
//获取集合name内所有的数据
MongoCursor<BasicDBObject> iterator = mongoTemplate.getCollection(name).find(BasicDBObject.class).iterator();
BasicDBObject object;
while (iterator.hasNext()) {
      object = iterator.next();
      //Do something
}
```

碰到的问题：

```
org.bson.codecs.configuration.CodecConfigurationException: Can't find a codec for class XXX.
```

解决方法：用BasicDBObject这个类来代替文档类型。