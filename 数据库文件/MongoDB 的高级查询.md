

### MongoDB的高级查询 **aggregate** 聚合管道 



主讲教师:(大地) 

合作网站:**www.itying.com** (IT 营) 我的专栏:https://www.itying.com/category-79-b0.html 

一、 MongoDB 聚合管道(Aggregation Pipeline)..............................................................................1 

二、 MongoDB Aggregation 管道操作符与表达式...............................................................................2 

三、 数据模拟........................................................................................................................................4 

四、 $project...........................................................................................................................................4 

五、 $match............................................................................................................................................5 

六、 $group.............................................................................................................................................6 

七、 $sort................................................................................................................................................6 

八、 $limit...............................................................................................................................................7 

九、 $skip................................................................................................................................................8 

十、 $lookup 表关联.............................................................................................................................9 



一、**MongoDB** 聚合管道(**Aggregation Pipeline**) 

使用聚合管道可以对集合中的文档进行变换和组合。 **实际项目:**表关联查询、数据的统计。 

MongoDB 中使用 db.COLLECTION_NAME.**aggregate([{<stage>},...])** 方法 来构建和使用聚合管道。先看下官网给的实例，感受一下聚合管道的用法。 

二、**MongoDB Aggregation** 管道操作符与表达式 

| 管道操作 符 | Description                                       |
| ----------- | ------------------------------------------------- |
| $project    | 增加、删除、重命名字段                            |
| $match      | 条件匹配。只满足条件的文档才能进入下一阶段        |
| $limit      | 限制结果的数量                                    |
| $skip       | 跳过文档的数量                                    |
| $sort       | 条件排序。                                        |
| $group      | 条件组合结果 统计                                 |
| $lookup     | $lookup 操作符 用以引入其它集合的数据(表关联查询) |

 

**SQL** 和 **NOSQL** 对比**:** 

| NOSQL    | SQL      |
| -------- | -------- |
| $match   | WHERE    |
| $group   | GROUP BY |
| $match   | HAVING   |
| $project | SELECT   |
| $sort    | ORDER BY |
| $limit   | LIMIT    |
| $sum     | SUM()    |
| $sum     | COUNT()  |
| $lookup  | join     |



**管道表达式:**

管道操作符作为“键”,所对应的“值”叫做管道表达式。 

例如 {$match:{ status:"A" }}, $Match 称为管道操作符，而 status:"A"称为管道表达式， 是管道操作符的操作数(Operand)。 

每个管道表达式是一个文档结构，它是由字段名、字段值、和一些表达式操作符组成的。 

| 常用表达式操作符 | **Description**        |
| ---------------- | ---------------------- |
| $addToSet        | 将文档指定字段的值去重 |
| $max             | 文档指定字段的最大值   |
| $min             | 文档指定字段的最小值   |
| $sum             | 文档指定字段求和       |
| $avg             | 文档指定字段求平均     |
| $gt              | 大于给定值             |
| $lt              | 小于给定值             |
| $eq              | 等于给定值             |

 

三、 数据模拟 , 将以下数据输入 mongoDB 数据库，以便进行模拟

```js
db.order.insert({"order_id":"1","uid":10,"trade_no":"111","all_price":100,"all_num":2}); db.order.insert({"order_id":"2","uid":7,"trade_no":"222","all_price":90,"all_num":2}); db.order.insert({"order_id":"3","uid":9,"trade_no":"333","all_price":20,"all_num":6}); 

db.order_item.insert({"order_id":"1","title":"商品鼠标 1","price":50,num:1}); db.order_item.insert({"order_id":"1","title":"商品键盘 2","price":50,num:1}); db.order_item.insert({"order_id":"1","title":"商品键盘 3","price":0,num:1}); 

db.order_item.insert({"order_id":"2","title":"牛奶","price":50,num:1}); db.order_item.insert({"order_id":"2","title":"酸奶","price":40,num:1}); 

db.order_item.insert({"order_id":"3","title":"矿泉水","price":2,num:5}); db.order_item.insert({"order_id":"3","title":"毛巾","price":10,num:1}); 
```



四、 **$project** 

修改文档的结构，可以用来重命名、增加或删除文档中的字段。 

要求查找 order 只返回文档中 trade_no 和 all_price 字段 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } } 

]) 
```



五、 **$match** 

作用
 用于过滤文档。用法类似于 find() 方法中的参数。 

```js
db.order.aggregate([ 
{ 

     $project:{ trade_no:1, all_price:1 } // 只查找两个字段
}, 

{
    $match:{"all_price":{$gte:90}}  // 再查找出 all_price 大于等于 90 的数据

} 
]) 
```



六、 **$group** 

```
将集合中的文档进行分组，可用于统计结果。
统计每个订单的订单数量，按照订单号分组
```

```js
db.order_item.aggregate( [ 
{
 $group: {_id: "$order_id", total: {$sum: "$num"}} 

} ] 

) 
```



七、 **$sort** 

将集合中的文档进行排序。 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } },  // 查询两个字段

{
 $match:{"all_price":{$gte:90}} // 匹配出 大于等于90的数据

}, { 

$sort:{"all_price":-1} } // 再按照从大到小的降序排序，

]) 
```



八、 **$limit** 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } }, 

{
 $match:{"all_price":{$gte:90}} 

}, { 

$sort:{"all_price":-1} }, 

{
 $limit:1 

} ]) 
```



九、 **$skip** 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } }, 

{
 $match:{"all_price":{$gte:90}} 

}, { 

$sort:{"all_price":-1} }, 

{
 $skip:1 

} ]) 
```



十、**$lookup** 表关联查询，非常重要哦

```js
db.order.aggregate([ {  // order 声明为主表
  $lookup: {  // 关联查询的命令
    from: "order_item",  // 声明要关联的表示 order_item
    localField: "order_id",  // 声明 主表字段
    foreignField: "order_id", // 声明要匹配的 关联表 字段
    as: "items" // 将关联结果放到 items 数组里
   } } 
]) 



// 以下是查询出的结果

{
 "_id": ObjectId("5b743d8c2c327f8d1b360540"), "order_id": "1",
 "uid": 10,
 "trade_no": "111",
 "all_price": 100,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743d9c2c327f8d1b360543"), "order_id": "1",
 "title": "商品鼠标 1",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da12c327f8d1b360544"), "order_id": "1",
 "title": "商品键盘 2",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b74f457089f78dc8f0a4f3b"), "order_id": "1",
 "title": "商品键盘 3",
 "price": 0, 

"num": 1 }] 

}{
 "_id": ObjectId("5b743d902c327f8d1b360541"), "order_id": "2",
 "uid": 7,
 "trade_no": "222",
 "all_price": 90,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743da52c327f8d1b360545"), "order_id": "2",
 "title": "牛奶",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da92c327f8d1b360546"), "order_id": "2", 

"title": "酸奶", "price": 40, "num": 1 

}] }{ 

"_id": ObjectId("5b743d962c327f8d1b360542"), "order_id": "3",
 "uid": 9,
 "trade_no": "333", 

"all_price": 20, "all_num": 6, "items": [{ 

"_id": ObjectId("5b743dad2c327f8d1b360547"), "order_id": "3",
 "title": "矿泉水",
 "price": 2, 

"num": 5 }, { 

"_id": ObjectId("5b743dff2c327f8d1b360548"), "order_id": "3",
 "title": "毛巾",
 "price": 10, 

"num": 1 }] 

} 
```



```js
db.order.aggregate([ { 

$lookup: 

{ 

} }, 

{
 $match:{"all_price":{$gte:90}} 

} 

]) 

{
 "_id": ObjectId("5b743d8c2c327f8d1b360540"), "order_id": "1",
 "uid": 10,
 "trade_no": "111",
 "all_price": 100,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743d9c2c327f8d1b360543"), "order_id": "1",
 "title": "商品鼠标 1",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da12c327f8d1b360544"), "order_id": "1",
 "title": "商品键盘 2",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b74f457089f78dc8f0a4f3b"), "order_id": "1",
 "title": "商品键盘 3",
 "price": 0, 

"num": 1 }] 

from: "order_item", 

localField: "order_id", foreignField: "order_id", as: "items" 

}{
 "_id": ObjectId("5b743d902c327f8d1b360541"), "order_id": "2",
 "uid": 7,
 "trade_no": "222",
 "all_price": 90,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743da52c327f8d1b360545"), "order_id": "2",
 "title": "牛奶",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da92c327f8d1b360546"), "order_id": "2",
 "title": "酸奶",
 "price": 40, 

"num": 1 }] 

} 
```





```js
db.order.aggregate([ 

{
 $lookup: 

{ 

} }, 

{
 $project:{ trade_no:1, all_price:1,items:1 } 

}, { 

$match:{"all_price":{$gte:90}} }, 

{
 $sort:{"all_price":-1} 

from: "order_item", 

localField: "order_id", foreignField: "order_id", as: "items" 

}, ]) 

{
 "_id": ObjectId("5b743d8c2c327f8d1b360540"), "trade_no": "111",
 "all_price": 100,
 "items": [{ 

"_id": ObjectId("5b743d9c2c327f8d1b360543"), "order_id": "1",
 "title": "商品鼠标 1",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da12c327f8d1b360544"), "order_id": "1",
 "title": "商品键盘 2",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b74f457089f78dc8f0a4f3b"), "order_id": "1",
 "title": "商 品键盘 3",
 "price": 0, 

"num": 1 }] 

}{
 "_id": ObjectId("5b743d902c327f8d1b360541"), "trade_no": "222",
 "all_price": 90,
 "items": [{ 

"_id": ObjectId("5b743da52c327f8d1b360545"), "order_id": "2",
 "title": "牛奶",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da92c327f8d1b360546"), "order_id": "2",
 "title": "酸奶",
 "price": 40, 

"num": 1 

}] }  
```









