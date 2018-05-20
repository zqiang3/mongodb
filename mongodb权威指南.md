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

### 创建

new_doc = {'name': 'zq', 'age': 12}

db.new_collection.insert(new_doc)

### 读取

db.new_collection.find()

db.new_collection.findOne()

### 更新

new_doc = {'name': 'zq', 'age': 12}

new_doc.age = 20

db.new_collection.update({'name': 'zq'}, new_doc)

### 删除

db.new_collection.remove({'name': 'zq'})