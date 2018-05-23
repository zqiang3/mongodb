# 第一章 MongoDB简介

## mongodb的特性

1. 非关系型数据库
2. 面向文档（文档内可再嵌入文档或数组）
3. 不需要预定义模式（可方便地添加或删除字段，使扩展性增加）
4. 易于扩展（纵向扩展与横向扩展，mongodb的设计采用横向扩展）
5. 高性能



# 第二章 MongoDB基础知识

## 文档

文档是mongodb的核心概念。在mongodb中，文档是键值对的有序集。

不要使用特殊字符：\0 . $

文档不能有重复的键。

键值对是有序的。

## 集合

集合内可放置任意文档，这是与关系型数据库显著的不同。

虽然mongodb的一个集合内可放置任何文档，但通常会将同种类型的文档放在一个集合里，这样会使数据更加集中，操作也更加方便。

## 数据库

每个数据库都有独立的权限

在磁盘上，每个数据库也放置在不同的文件中。

数据库最终会以文件系统中的文件存在，而数据库名就是相应的文件名，因此数据库名称有很多的限制。

## MongoDB shell

**创建**

new_doc = {'name': 'zq', 'age': 12}

db.new_collection.insert(new_doc)

**读取**

db.new_collection.find()

db.new_collection.findOne()

**更新**

new_doc = {'name': 'zq', 'age': 12}

new_doc.age = 20

db.new_collection.update({'name': 'zq'}, new_doc)

**删除**

db.new_collection.remove({'name': 'zq'})

**使用shell连接mongo实例**

```shell
mongo 127.0.0.1:27017/test -uuser -ppwd
# 不连接数据库
mongo --nodb

conn = new Mongo('127.0.0.1:27017')
db = conn.getDB('test')
db.auth('user', 'pwd')
```

**shell帮助**

```javascript
help
db.help()
db.coll.help()
show dbs
show collections
show users
db.coll.update  // 查看函数的实现代码
```

**.mongorc.js文件**

这个文件会在启动shell时自动运行。

可在.mongorc.js里重写某些方法，移除危险函数

```javascript
var no = function(){
    print("Not on my watch.");
};

db.dropDatabase = DB.prototype.dropDatabase = no;  // 禁止删除数据库
DBCollection.prototype.drop = no;                  // 禁止删除集合
DBCollection.prototype.dropIndex = no;             // 禁止删除索引
```

如果在启动shell时指定--norc参数，就可以禁止加载.mongorc.js

**定制shell提示**

**编辑复合变量**

## 数据类型

* null
* 布尔型：true和false
* 数值：默认使用64位浮点型数值
* 字符串
* 日期：新纪元以来经过的毫秒数，不存储时区
* 正则表达式
* 数组：{"x": ["a", "b", "c"]}
* 内嵌文档：{"x": {"foo": "bar"}}
* 对象id：是一个12字节的ID，文档的唯一标识。
* 二进制数据：将非UTF-8字符保存到数据库，二进制数据是唯一的方式。
* 代码：可包含JavaScript代码

## _id和ObjectId

[ObjectId 官方链接](https://docs.mongodb.com/manual/reference/bson-types/#objectid)

**ObjectId**

ObjectId使用12字节的存储空间，每个字节两位十六进制数字，是一个24位的字符串。

前4字节是从标准纪元开始的**时间戳**。

接下来3个字节是主机的唯一标识符，通常是主机名的散列值。

接下来2个字节是产生ObjectId的进程**PID**，确保同一台机器并发产生的id是唯一的。

最后3个字节是自增计数器，确保相同进程同一秒钟产生的id是唯一的。

**自动生成id**

ObjectId是由客户端生成的！如果插入文档的时候没有”_id”键，系统会帮你自动创建一个。可以由MongoDB服务器来做这件事情，但通常会在客户端由驱动程序完成。

**根据_id查看文档创建时间**

```javascript
ObjectId("577d025056f08b9cdd727d97").getTimestamp()
```

# 第三章

## 插入文档

本书版本的mongodb单条消息最大数据量限制为48M

### 批量插入

```javascript
db.products.insert(
   [
     { _id: 11, item: "pencil", qty: 50, type: "no.2" },
     { item: "pen", qty: 20 },
     { item: "eraser", qty: 25 }
   ]
)
```



### insert和save的区别

如果插入文档的_id值，在collection中已经存在，使用insert插入将会报异常，用save操作会覆盖掉原来的值。