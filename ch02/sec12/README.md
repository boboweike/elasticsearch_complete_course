获取文档（通过id）
===

## 通过id获取单个文档

```
GET /books/_doc/1
```

```json
{
  "_index" : "books",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "title" : "Effective Java",
    "Author" : "Joshua Bloch",
    "Rating" : 4.7,
    "Price" : 60,
    "inStock" : 300
  }
}
```

查询不存在的文档
```
GET /books/_doc/999
```

```json
{
  "_index" : "books",
  "_type" : "_doc",
  "_id" : "999",
  "found" : false
}
```

检查文档是否存在
```
HEAD /books/_doc/999
```

```json
{
  "statusCode":404,
  "error":"Not Found",
  "message":"404 - Not Found"
}
```

## 从单个索引中获取多个文档

```json
GET books/_mget
{
  "ids": ["1", "2", "3", "999"]
}
```

## 从多个索引中获取多个文档
```json
GET _mget
{
  "docs": [
    {
      "_index": "movies",
      "_id": 100
    },
    {
      "_index": "books",
      "_id": 1
    },
    {
      "_index": "books",
      "_id": 2
    }
  ]
}
```

**注意**：_id属性暂不接受数组

## 通过查询获取多个文档
```json
GET books/_search
{
  "query": {
    "ids": {
      "values": [1,2,3]
    }
  }
}

GET movies,books/_search
{
  "query": {
    "ids": {
      "values": [1,2,3,100]
    }
  }
}
```




