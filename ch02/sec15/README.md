通过脚本更新文档
===

## 查看现有文档
```json
GET /_cat/indices?v

GET /books/_search

GET /books/_doc/1
```

## 更新某个字段

* 将字段inStock减1
```json
POST /books/_update/2
{
  "script": {
    "source": "ctx._source.inStock--"
  }
}

* 给字段price赋予某个值
POST /books/_update/2
{
  "script": {
    "source": "ctx._source.price = 65"
  }
}

* 更新数组字段
POST /books/_update/2
{
  "script": {
    "source": "ctx._source.tags.add('classic')"
  }
}

POST /books/_update/2
{
  "script": {
    "source": "ctx._source.tags.remove(ctx._source.tags.indexOf('classic'))"
  }
}
```

## 条件更新脚本
```json
POST /books/_update/2
{
  "script": {
    "source": """
      if (ctx._source.Rating >= 4.5) {
        ctx._source.good_rating = true
      } else {
        ctx._source.good_rating = false
      }
    """
  }
}

* 移除某个字段
POST /books/_update/2
{
  "script": {
    "source": "ctx._source.remove('good_rating')"
  }
}
```

## 向脚本传入参数
```json
POST /books/_update/2
{
  "script": {
    "source": "ctx._source.inStock -= params.quantity",
    "params": {
      "quantity": 10
    }
  }
}
```
优势：一次编译，多次重用，提升性能

## 总结：通过脚本更新文档的基本语法

```json
POST /<index>/_update/<id>
{
  "script" : {
    "source": "...",  // 条件/表达式和给文档设置的值
    "lang": "painless|expression..", // 表达式语言，缺省是painless
    "params": {...} // 为脚本传入动态数据
  }
}
```



