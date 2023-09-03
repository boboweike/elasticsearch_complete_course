## 创建索引

### 1. 索引 Index 配置的三个组成部分

```json
DELETE books

PUT /books/_doc/1
{
  "book_id": 1,
  "book_name": "Spring in Action, Sixth Edition",
  "rating": 4.3,
  "price": 75,
  "author": "Craig Walls",
  "pub_date": "2022-03-01",
  "publisher": "Manning"
}

GET books
```

### 2. 关闭和开启自动索引创建

```json
GET _cluster/settings

PUT _cluster/settings
{
  "persistent": {
    "action.auto_create_index": false
  }
}

PUT _cluster/settings
{
  "persistent": {
    "action.auto_create_index": true
  }
}
```

### 3. 索引的配置 ~ 静态 vs 动态

- 静态配置：创建索引时候设置，不能在活跃索引上修改的配置。
  - number of shards ~ `shard_home = hash(doc_ID) % number_of_shards`
  - compression codec
  - etc
- 动态配置：可以在活跃索引上配置和修改
  - number of replicas
  - allowing or disallowing writes
  - refresh intervals
  - etc

```json
PUT books_with_custom_settings
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 5,
    "codec": "best_compression",
    "max_script_fields": 128,
    "refresh_interval": "60s"
  }
}

GET books_with_custom_settings/_settings
GET books_with_custom_settings/_settings/index.number_of_shards

PUT books_with_custom_settings/_settings
{
  "settings": {
    "number_of_shards": 5
  }
}

PUT books_with_custom_settings/_settings
{
  "settings": {
    "number_of_replicas": 2
  }
}
```
