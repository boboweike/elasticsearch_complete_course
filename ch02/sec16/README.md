Upsert(Update or Insert)
===

## Upsert

Doc exists? YES -> 执行脚本 : NO -> 插入一个新文档

```json
// 文档不存在，第一次执行结果created
// 文档存在，第二次执行结果updated
POST /books/_update/20
{
  "script": {
    "source": "ctx._source.inStock++"
  },
  "upsert": {
    "title": "Kubernetes in Action",
    "authtor": "Marko Luksa",
    "price": 99,
    "inStock": 50
  }
}

GET /books/_doc/20
```

## Doc as upsert
``` json
// 文档不存在，报错
POST /movies/_update/10
{
  "doc": {
    "title": "Avatar",
    "director": "James Cameron"
  }
}

// 文档不存在则插入
POST /movies/_update/10
{
  "doc": {
    "title": "Avatar",
    "director": "James Cameron"
  },
  "doc_as_upsert": true
}

GET /movies/_doc/10
```


