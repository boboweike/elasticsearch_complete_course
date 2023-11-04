# 存在(Exists)检查

类似数据库中的`select * from movies where tag is NOT NULL`

空值包括: NULL, []，注意空字符串""不是空值

空值原因：

1. 插入时没有提供值(主要原因)
2. 索引 mapping 中，某个字段的 index 被设置为 false，例如时间序列数据，用于聚合
3. 值的长度超过了 ignore_above 参数的阈值(动态映射 keyword 缺省 256)
4. 索引 mapping 中，ignore_malformed 参数被设为 true

## 1. 查询某个字段存在的文档

```json
GET movies/_search
{
  "query": {
    "exists": {
      "field": "actors"
    }
  }
}
```

## 2. 查询某个字段不存在的文档

```json
# 需使用bool复合查询
GET movies/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "exists": {
            "field": "release_date"
          }
        }
      ]
    }
  }
}
```

## 3. 下节课 ～ 范围(range)查询
