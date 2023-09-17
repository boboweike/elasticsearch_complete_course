# 读取/打开/关闭/删除索引

## 读取索引

```json
GET books1,books2,books3
GET books1,books2, books3
GET books*

HEAD books_new

GET books1/_settings
GET books1/_mapping
GET books1/_alias

GET *
GET _all
```

## 关闭和打开索引

```json
POST books1/_close
PUT books1/_doc/3
{
  "title": "Head First Java"
}
POST books1/_open
POST books*/_close

GET _cluster/settings
GET _cluster/settings
{
  "persistent": {
    "cluster.indices.close.enable": false
  }
}
```

## 删除索引

```json
DELETE books1/_alias/wildcard_books_alias
GET books1/_alias
DELETE books1
DELETE books*
```
