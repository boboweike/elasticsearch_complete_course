# Reindex 高级用法

## 通过查询过滤文档

```json
POST /_reindex
{
  "source": {
    "index": "books",
    "query": {
      "range": {
        "rating": {
          "gte": 4.9
        }
      }
    }
  },
  "dest": {
    "index": "books_new"
  }
}
```

## 通过脚本过滤文档

```json
POST /_reindex
{
  "source": {
    "index": "books"
  },
  "dest": {
    "index": "books_new"
  },
  "script": {
    "source": """
    if (ctx._source.rating < 4.8) {
      ctx.op = "noop";
    }
    """
  }
}
```

## 移除部分字段

```json
POST /_reindex
{
  "source": {
    "index": "books",
    "_source": ["author", "title", "price"]
  },
  "dest": {
    "index": "books_new"
  }
}
```

## 重命名字段

```json
POST /_reindex
{
  "source": {
    "index": "books"
  },
  "dest": {
    "index": "books_new"
  },
  "script": {
    "source": """
    ctx._source.book_name = ctx._source.remove("title")
    """
  }
}
```
