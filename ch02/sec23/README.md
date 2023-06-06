文档删除
===

方法1: Delete by id
方法2: Delete by query

## 1. Delete by id
```json
DELETE books/_doc/3
```

如果文档不存在，result = not_found(404)

## 2. Delete by query

_delete_by_query和_update_by_query类似，用到Snapshot/Primary_term/SeqNo和Batch

```json
POST books/_delete_by_query
{
  "query": {
    "match": {
      "author": "Marko Luksa"
    }
  }
}
```

## 3. Delete by range query
```json
POST books/_delete_by_query
{
  "query": {
    "range": {
      "price": {
        "gt": 50,
        "lt": 60
      }
    }
  }
}
```

## 4. Delete all docs in one index
```json
POST books/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

## 5. Delete all docs in multiple indexes
```json
GET /_cat/indices?v

POST books_new,movies/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```