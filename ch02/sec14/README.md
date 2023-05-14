更新(Update)文档
===

## 修改现有文档的字段

```json
GET /books/_search

POST /books/_update/3
{
  "doc": {
    "inStock": 180
  }
}

GET /books/_doc/3
```

结果
```json
{
  "_index" : "books",
  "_type" : "_doc",
  "_id" : "3",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 3,
    "successful" : 3,
    "failed" : 0
  },
  "_seq_no" : 8,
  "_primary_term" : 4
}
```
**注意**，版本号递增了

再次做相同更新 -> noop
```json
{
  "_index" : "books",
  "_type" : "_doc",
  "_id" : "3",
  "_version" : 2,
  "result" : "noop",
  "_shards" : {
    "total" : 0,
    "successful" : 0,
    "failed" : 0
  },
  "_seq_no" : 8,
  "_primary_term" : 4
}
```

### 给文档添加新字段
```json
POST /books/_update/3
{
  "doc": {
    "tags": ["java", "programming"]
  }
}
```

结果：
```json
{
  "_index" : "books",
  "_type" : "_doc",
  "_id" : "3",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 3,
    "successful" : 3,
    "failed" : 0
  },
  "_seq_no" : 9,
  "_primary_term" : 4
}
```
查询+结果：
```json
GET /books/_doc/3
```

### 更新原理

* 原理：1.获取和id对应的现有文档 -> 2.更新字段 -> 3.替换(replace)现有文档
  * 原文档不可变(immutable)
  * 对更新的文档进行重新索引
  * 将原文档标记为删除
* 可手动实现，但是需要两次API调用
* Update API只需要一次调用，自动在对应的shard上进行操作

手动使用PUT/replace实现
```json
GET /books/_doc/3

PUT /books/_doc/3
{
    "title" : "Thinking in Java",
    "Author" : "Bruce Eckel",
    "Rating" : 4.5,
    "Price" : 60,
    "inStock" : 160,
    "tags": ["java", "programming"]
}
GET /books/_doc/3
```




