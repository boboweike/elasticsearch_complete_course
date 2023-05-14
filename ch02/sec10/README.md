第二章，ES文档管理
===

## 1. ES文档管理API

* 文档插入索引(document indexing)API ～ 将文档插入存储到ES的索引
* 文档读取和查询API ～ 从ES中读取或者查询文档
* 文档更新API ~ 更新文档中的字段
* 文档删除API ~ 将文档从ES存储中删除

API分为两类：
* 单文档操作API
* 多文档操作API

## 2 文档索引


### 2.1 文档标识符(identifier)

* 每一个ES文档都有一个索引内唯一的标识符(identifier)，类似关系数据库中的主键
* 标识符由用户提供 ～ 使用HTTP PUT操作添加，例如将关系数据库中数据导入ES
* 标识符有ES自动生成 ～ 使用HTTP POST操作添加，UUID，例如将网站日志导入ES


### 2.2 带标识符的PUT演示

删除+新建books索引：
```
GET /_cat/indices?v

DELETE books

PUT /books
{
  "settings": {
    "index": {
      "number_of_shards": 2,  
      "number_of_replicas": 2 
    }
  }
}
```

添加一本带id的书：
```
PUT books/_doc/1
{
  "title": "Thinking in Java",
  "Author": "Bruce Eckel",
  "Rating": 4.5,
  "Price": 79,
  "inStock": 200
}
```

小技巧 ～ 从Kibana DevConsole获取curl命令
```
curl -XPUT "http://es01:9200/books/_doc/1" -H 'Content-Type: application/json' -d'{  "title": "Thinking in Java",  "Author": "Bruce-Eckel",  "Rating": 4.5,  "inStock": 200}'
```

结果`created`:
```json
{
  "_index" : "books",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 3,
    "successful" : 3,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 3
}
```

再次执行相同命令，结果`updated`，版本号递增。

### 2.3 不带标识符的POST演示

添加一本不带id的书：
```
POST books/_doc
{
  "title": "Effective Java",
  "Author": "Joshua Bloch",
  "Rating": 4.7,
  "Price": 60,
  "inStock": 300
}
```

结果`created`:
```json
{
  "_index" : "books",
  "_type" : "_doc",
  "_id" : "JKwk0IcBlN1ZfssWTy_Z",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 3,
    "successful" : 3,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

再次执行相同命令，结果生成一个新的文档。

### 2.4 演示～PUT使用_create避免文档覆盖问题

```
PUT books/_create/1
{
  "title": "Thinking in Java",
  "Author": "Bruce Eckel",
  "Rating": 4.5,
  "Price": 60,
  "inStock": 200
}
```

结果 ～ 版本冲突

添加一本新书：
```
PUT books/_create/2
{
  "title": "Head First Java",
  "Author": "Kathy Sierra",
  "Rating": 4.5,
  "Price": 80,
  "inStock": 1000
}
```
添加成功。

重复添加～版本冲突

## 3. 关于自动创建索引

`action.auto_create_index`，缺省为`true`

```
PUT /movie/_doc/100
{
  "title": "Mission Impossible",
  "director": "James Cameron"
}
```

```
GET /_cluster/settings

PUT /_cluster/settings
{
    "persistent": {
        "action.auto_create_index": "false" 
    }
}
```

## 4. 小结

1. 添加不带id的文档 ～ POST {index}/_doc
2. 添加带id的文档 ~ PUT {index}/_doc/{id}
3. 避免文档被覆盖 ~ put {index}/_create/{id}

下节课内容预览： 添加文档时ES底层是如何工作的？




