批量(bulk)操作
===

## 批量索引(index)

```json
POST _bulk
{"index": {"_index": "movies", "_id": "10"}}
{"title": "Mission Impossible", "release_date": "1996-07-05"}
```
存在则replace

## 批量索引(create)
```json
POST _bulk
{"create": {"_index": "movies", "_id": "10"}}
{"title": "Mission Impossible", "release_date": "1996-07-05"}
```
存在不会覆盖

### 索引名称在请求URL中

```json
POST movies/_bulk
{"index": {"_id": "11"}}
{"title": "Mission Impossible II","release_date": "2000-05-24"}
```

### 让ES自动生成id

```json
POST movies/_bulk
{"index":{}}
{"title": "Mission Impossible III", "release_date": "2006-05-03"} 
```

### 更新Update
```json
POST _bulk
{"update": {"_index": "movies", "_id": "10"}}
{"doc": {"director":"Brian De Palma", "actors":["Tom Cruise", "Emmanuelle Béart"]}}
```

### 删除Delete
```json
POST _bulk 
{"delete":{"_index":"movies","_id":"11"}}
```

### 批量索引Tom Cruse的电影

为什么要Bulk?

```json
POST movies/_bulk
{"index":{}}
{"title": "Mission Impossible","release_date": "1996-07-05"}
{"index":{}}
{"title": "Mission Impossible II","release_date": "2000-05-24"}
{"index":{}}
{"title": "Mission Impossible III","release_date": "2006-05-03"} 
{"index":{}}
{"title": "Mission Impossible - Ghost Protocol","release_date": "2011-12-26"}
```


### 批量操作不同的索引
```json
POST _bulk
{"index": {"_index":"books"}}
{"title": "Kubernetes in Action"} 
{"create": {"_index":"flights", "_id":"11"}}
{"title": "Shanghai to Beijing"} 
{"index":{"_index":"pets"}}
{"name": "Kitty","age_months": 2} 
{"delete": {"_index":"movies", "_id":"11"}}
{"update": {"_index":"movies", "_id":"10"} }
{"doc": {"title" : "Forrest Gump"} }

```

## Bulk API使用场景
* 大批量数据的一次性导入或者更新
* 减少网络round trip开销，性能更好
* 通常通过程序/脚本自动生成Bulk requests

## 关于Bulk API的注意事项
* Content-Type: application/x-ndjson
  * new line delimited JSON
  * application/json可接受，但并不是正确方式
  * Kibana Dev Console/ES SDK会自动添加
  * HTTP Client/Postman需要手动添加
  * cURL也需要手动添加(下节课演示)
* 每一行json文档`必须`以新行(`\n`或者`r\n`)结尾
  * 包括最后一行(经常容易犯错)
  * 如果你的批量插入文档是通过程序生成，也需要注意最后一个空行
  * 普通编辑器中每行按回车即可
  * Kibana Dev Console会自动添加
* 单个操作失败，并不会影响bulk的整体操作
  * Bulk API会返回每一个操作的结果
  * 通过items，检查某个某个操作是否有问题
  * 结果顺序和操作顺序相同
  * errors字段表明是否有错误
* 支持乐观并发操作
  * 在元数据中使用`if_primary_term`和`if_seq_no`参数
