# 高级索引操作～ shink API

```json
# shrink API目的：减少shards数量

PUT all_movies
{
  "settings": {
    "number_of_shards": 10
  }
}

GET all_movies

PUT all_movies/_doc/1
{
  "title": "A test movie"
}

# 禁用写入(只读)
PUT all_movies/_settings
{
  "settings": {
    "index.blocks.write": true
  }
}

PUT all_movies/_shrink/all_movies_new
{
  "settings": {
    "index.blocks.write": null,
    "index.number_of_shards": 2
  }
}

GET all_movies_new

PUT all_movies_new/_doc/2
{
  "title": "A test movie 2"
}

# 关于shrink API的补充说明：
# 1. 源索引必须禁用写入(只读)
# 2. 操作前，目标索引必须先不存在的
# 3. 目标索引的shards数量，必须小于等于源索引shards数量
# 4. 目标索引的shards数量，必须可以被源索引shards数量整除(50 -> 25, 10, 5, 1)
# 5. 存储索引的节点上必须有足够存储空间
```
