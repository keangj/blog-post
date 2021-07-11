---
title: MongoDB
date: 2019-04-05 20:17:14
tags:
---

# mongoDB

## 操作数据库

- 创建、切换  
    `use 数据库名`  
    切换数据库，如不存在就创建一个新的数据库。

    <!-- ```sh
      use test
    ``` -->

- 删除  
    `db.dropDatabase()`  
    删除数据库
- 查看  
    `show dbs`
    查看当前的数据库

## 操作集合

- 查看集合  
    `show tables`  
    查看当前数据库中的集合
- 创建集合  
    `db.集合名.insert()`  
    向集合里插入文档，如集合不存在创建一个集合
- 删除集合  
    `db.集合名.drop()`  
    删除当前数据库中的集合

## 操作文档

- 查看集合里的文档  
    `db.集合名.find()`  
    查看集合中的全部文档
- 查看第一个文档  
    `db.集合名.find`

- 查看某个文档  
    `db.集合名.find({查询条件})`  
    查询符合条件的文档  
    `db.集合名.find({content: /jay/})`  
    查询 content 包含 'jay' 的文档  
    `db.集合名.find({content: /^jay/})`  
    查询 content 以 'jay' 开头的文档  
    `db.集合名.find({content: /jay$/})`  
    查询 content 以 'jay' 结尾的文档

- 插入文档  
    `db.集合名.insert(doc)`
- 更新文档(覆盖)  
    `db.集合名.update({查询条件}, {更新内容})`  
    更新符合条件的文档
- 更新文档(添加)  
    使用操作符 `$set`  
    `db.集合名.update({查询条件}, {操作符: {key: value}})`  
    修改某个 key 的 value，如不存在就添加
    `db.集合名.update({查询条件}, {$unset: {key: value}})`
    删除某个 key 和 value

## 操作符

|  条件操作符  |   意义  |
| ------ | ------ |
|$gt(greater than)|   大于|
|$gte(gt equal)|      大于等于|
|$lt(less than)|      小于|
|$lte(lt equal)|      小于等于|
|$eq(equal)|          等于|
|$ne(not eq)|         不等于|
|$in|                 包含|
|$nin|                不包含|

|  逻辑操作符         |   意义            |
| ---------- | ---------- |
|$exists|             存在|
|$not|                不存在|
|$or|                 或者|
|$and|                和|
|$mod|                求模|
|$where|              位置|

|   数组操作符   |   意义  |
|   -------    |    --  |
|   $size      | 匹配数组长度大小|
|   $all       | 匹配数组字段中包含指定  Value 的文档 |

## 常见查询

查询 content 包含 'jay' 的文档
`db.集合名.find({content: /jay/})`
查询 content 以 'jay' 开头的文档
`db.集合名.find({content: /^jay/})`
查询 content 以 'jay' 结尾的文档
`db.集合名.find({content: /jay$/})`
查询 content 存在 info 属性的文档
`db.集合名.find({'content.info': {exists: true}})`
查询 content 包含 jay 或者 kang 的文档
`db.集合名.find({content: {$in: ['jay', 'kang']}})`
查询 content 不包含 jay 或者 kang 的文档
`db.集合名.find({content: {$nin: ['jay', 'kang']}})`
查询符合条件文档中的 name 和 age 字段
`db.集合名.find({}, {name: true, age: 1})`
查询符合条件文档返回中不包含 name 和 age 字段
`db.集合名.find({}, {name: false, age: 0})`
查询 age 大于 18 的文档
`db.集合名.find({age: {$gt: 18}})`
`db.集合名.find({$where:'this.age > 18'})`
`db.集合名.find({'this.age > 18'})`
查询 age 大于等于 18 且小于等于 30 的文档
`db.集合名.find({age: {$gte: 18, $lte: 30}})`
查询 name 不为 'jay' 的文档
`db.集合名.find({name: {$ne: 'jay'}})`
查询 date 大于 2019 年 1 月 1 日，小于 2019 年 12 月 31 日的文档
`db.集合名.find({date: {$gt: new Date(2019,0,1), $lt: new Date(2019,11,31)}})`
查询文档数量
`db.集合名.find().count()`
查询 age 按降序排列的文档
`db.集合名.find().sort({age: -1})`
查询前 5 条文档
`db.集合名.find().limit(5)`
查询 5 条之后的文档
`db.集合名.find().skip(5)`
查询第 10 条到第 20 条文档
`db.集合名.find().limit(20).skip(10)`