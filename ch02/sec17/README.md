通过查询进行更新(Update by query)
===

* 之前学习的是单个文档的更新
* 如何实现对满足查询条件的多个文档的一次性更新？
  * 类似数据库中的`UPDATE WHERE`

## Update by query
```json
POST /books/_update_by_query
{
  "script": {
    "source": "ctx._source.inStock += 10"
  },
  "query": {
    "match_all": {}
  }
}

GET /books/_search
{
  "query": {
    "match_all": {}
  }
}
```

结果
```json
{
  "took" : 30,
  "timed_out" : false,
  "total" : 9, // 文档总数
  "updated" : 9, // 更新文档数
  "deleted" : 0,
  "batches" : 1, // 获取文档的批数
  "version_conflicts" : 0, // 版本冲突次数
  "noops" : 0,
  "retries" : { // 重试
    "bulk" : 0, // 批量更新重试次数
    "search" : 0 // 查询重试次数
  },
  "throttled_millis" : 0,
  "requests_per_second" : -1.0,
  "throttled_until_millis" : 0,
  "failures" : [ ] // 失败信息
}
```

## 为什么要引入快照Snapshot

* 防止保存快照后数据被再次修改
  * query/update可能涉及很多文档，需要花费一定时间，期间数据可能被外部请求更新(Create/Update/Delete)
* 通过快照确保文档的Primary term和顺序号(sequence number)没有变化
  * 只有当Primary term和sequence number相同时，文档才会更新
  * 乐观并发控制
* 冲突的数量在`version_conflicts`结果字段中返回

## 冲突时继续处理
```json
POST /books/_update_by_query
{
  "conflicts": "proceed",
  "script": {
    "source": "ctx._source.inStock++"
  },
  "query": {
    "match_all": {}
  }
}
```
冲突只会被计数，但是更新会继续(不会终止)

## 小结
* update by query使用快照实现乐观并发控制
* Query + Bulk update在多个Replication groups中是顺序执行的
  * 如果一个失败，重试10次
  * 如果重试后仍然失败，整个查询更新被终止
  * 已经更新的文档不会回滚(不是事务性更新)
  * 失败信息在结果failures字段中被返回

## 新概念
* Primary terms
* 顺序号(Sequence numbers)
* 乐观并发控制(Optimistic concurrency control)

后续内容：路由/文档读取和写入的原理，文档版本和乐观并发控制
