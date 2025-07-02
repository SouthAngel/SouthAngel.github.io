---
title: IndexDB
---

IndexDB 是现代浏览器提供的一个数据库，会把数据记录在本地，可以看作是 LocalStorge 的升级版，与LocalStorge不同的是，IndexDB 允许收录更大量的数据（取决于你的本地硬盘容量），并且还支持事务与索引，提供更高级的查询功能。
- IndexDB 依然受同源策略限制，不能跨域访问
- IndexDB 支持异步
- IndexDB 更像NoSQL类型数据库，使用键值对储存，不支持SQL语句

## 创建/打开数据库
```js
let databaseName = "testdb";
let request = window.indexedDB.open(databaseName);

var db;

request.onerror = function (event) {
  console.log('open db error');
};

request.onsuccess = function (event) {
    db = request.result;
    console.log('open db success');
};

```

## 创建/打开存储对象
```js
var data = [
    {"name":"fx_fire","content":"^`'``''"},
    {"name":"mod_stone","content":"0,1,4,2,9,14","description":""}
];
db.createObjectStore("collection1",{keyPath:"name"});
```

## 增删改查
```js
// add delete put get
request = db.transaction(['collection1'],'readwrite').objectStore('collection1').add(data[0]);
request.onerror = (e)=>{};
request.onsuccess = (e)=>{};
```

## 索引与游标
- 创建索引   objectStore.createIndex(index_name, index_field, {unique: true})
- 通过索引查询   objectStore.index(index_name).get(index_value)
- 创建游标 openCursor
- 迭代游标 cursor.continue
