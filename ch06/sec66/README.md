# 基于 ID 的查询

# 适用于 IDs 已知的情况，比方说文档的 IDs 存在数据库中，文档存在 ES 中。

## 1. 通过 IDs 查询多个文档

输入的 ID 可以是不存在的

```json
GET movies/_search
{
  "query": {
    "ids": {
      "values": [1,2,3,30]
    }
  }
}
```

## 2. 等价的 terms 查询

```json
GET movies/_search
{
  "query": {
    "terms": {
      "_id":[1,2,3,30]
    }
  }
}
```

## 3. 下一个文档存在(existence)检查
