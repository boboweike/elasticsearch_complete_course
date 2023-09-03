# 索引别名(aliases)

## 1. 别名的用途

- 通过单个别名从多个索引中查询和聚合数据
- 实现不停机重建索引(zero downtime during re-reindex)

## 2. 为单个索引创建别名

```json
// 创建索引时通过`aliases`对象创建别名
PUT books_for_aliases
{
  "aliases": {
    "my_new_books_alias": {}
  }
}

// 查看索引的别名
GET books_for_aliases/_alias

// 在现有索引上通过`_alias`端点创建别名
PUT books_for_aliases/_alias/my_books_alias

// 通过别名索引文档
PUT my_books_alias/_doc/1
{
  "title": "Java in Action"
}

// 通过别名查询文档
GET my_books_alias/_search
{
  "query": {
    "match": {
      "title": "java"
    }
  }
}
```

## 3. 创建单个别名同时指向多个索引

```json
PUT books1/_doc/1
{
  "title": "Head First Java"
}

PUT books2/_doc/1
{
  "title": "Head First Design Pattern"
}

PUT books3/_doc/1
{
  "title": "Head First Docker"
}

// 创建单个别名指向多个索引
PUT books1,books2,books3/_alias/multi_books_alias

// 通过别名查询文档
GET multi_books_alias/_search
{
  "query": {
    "match": {
      "title": "docker"
    }
  }
}

// 注意：如果通过别名写入文档，其中一个索引的is_write_index必须设置为true，否则报错
PUT multi_books_alias/_doc/2
{
  "title": "Head First Git"
}

POST /_aliases
{
  "actions": [
    {
      "add": {
        "index": "books1",
        "alias": "multi_books_alias",
        "is_write_index": true
      }
    }
  ]
}


// 通过wildcard创建别名
PUT books*/_alias/wildcard_books_alias

// 查询多个索引上的别名
GET books1,books2,books3/_alias
GET books*/_alias

```

## 4. 实现不停机重建迁移(zero downtime during re-reindex)

需求：将 `books_old` index 迁移到 `books_new` index

1. 为当前索引`books_old`创建一个别名`books_alias`
2. 将查询流量导向通过`books_alias`进行访问。迁移期间一般要求禁止写入，如果有写入可以暂时用队列(kafka)缓存
3. 创建一个新的索引`books_new`，`books_new`的 schema 可以和`books_old`不兼容
4. 通过 re-index API 将数据从老索引`books_old`迁移到`books_new`
5. 更新`books_alias`，将它从`books_old`更新为指向`books_new`
6. 此时查询流量被从`books_old`切换到`books_new`
7. 验证查询没有问题，如果有写入，打开写入，校验写入也没有问题
8. 备份或删除`books_old`

### 4.1 通过`_aliases` API 一次执行多个别名操作

```json
PUT books_old
{
  "aliases": {
    "books_alias": {}
  }
}

PUT books_new

GET books_new,books_old/_alias


POST _aliases
{
  "actions": [
    {
      "remove": {
        "index": "books_old",
        "alias": "books_alias"
      }
    },
    {
      "add": {
        "index": "books_new",
        "alias": "books_alias"
      }
    }
  ]
}
```
