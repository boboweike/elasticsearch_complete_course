# 高级索引操作～ split API

```json
GET /_cat/indices?v

# split目标：优化数据分布，提升查询性能

PUT all_books

GET all_books

PUT all_books/_doc/1
{
  "title": "Head First Design Pattern"
}

# 禁用写入(只读)
PUT all_books/_settings
{
  "settings": {
    "index.blocks.write": true
  }
}

PUT all_books/_doc/2
{
  "title": "Head First Java"
}

POST all_books/_split/all_books_new
{
  "settings": {
    "index.number_of_shards": 4
  }
}

GET all_books_new

# 1. 操作前目标索引必须不存在
# 2. number_of_shards必须是源索引中shards数量的倍数(3 -> 3/6/9)
# 3. 目标索引的primary shards数量 >= 源索引的primary shards数量
# 4. 目标索引所在数据节点必须有足够空间
# 5. 除了shards数量，其它配置(settings/mappings/alias)从源拷到目标索引


POST all_books_new/_split/all_books_newest
{
  "settings": {
    "index.number_of_shards": 5
  }
}
```
