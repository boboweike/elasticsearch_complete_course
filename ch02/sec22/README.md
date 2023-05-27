乐观并发控制
===

```json
GET /books/_search

GET /books/_doc/6

POST /books/_update/6?if_primary_term=8&if_seq_no=129
{
  "doc": {
    "inStock": 256
  }
}
```
* 第一次更新成功
* 第二次重复执行 -> version conflict