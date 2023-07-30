# 更新已有 Mapping

## 准备数据

```json
PUT /books
{
  "mappings": {
    "properties": {
      "book_id": {
        "type": "integer"
      },
      "title": {
        "type": "text"
      },
      "rating": {
        "type": "float"
      },
      "price": {
        "type": "integer"
      },
      "author": {
        "type": "text"
      },
      "pub_date": {
        "type": "date"
      },
      "publisher": {
        "type": "keyword"
      }
    }
  }
}


PUT /books/_doc/1
{
  "book_id": 101,
  "title": "Thinking in Java 4th Edition",
  "rating": 4.5,
  "price": 65,
  "author": "Bruce Eckel",
  "pub_date": "2006-02-10",
  "publisher": "Person"
}

PUT /books/_doc/2
{
  "book_id": 102,
  "title": "Effective Java 3rd Edition",
  "rating": 4.7,
  "price": 88,
  "author": "Joshua Bloch",
  "pub_date": "2017-11-27",
  "publisher": "Addison-Wesley"
}

PUT /books/_doc/3
{
  "book_id": 103,
  "title": "Head First Java: A Brain-Friendly Guide",
  "rating": 4.7,
  "price": 60,
  "author": "Kathy Sierra, Bert Bates, Trisha Gee",
  "pub_date": "2022-06-21",
  "publisher": "OReilly-Media"
}

PUT /books/_doc/4
{
  "book_id": 104,
  "title": "The Well-Grounded Java Developer, Second Edition",
  "rating": 4.9,
  "price": 99,
  "author": "Benjamin Evans, Martijin Verburg, Jason Clark",
  "pub_date": "2022-12-22",
  "publisher": "Manning"
}

PUT /books/_doc/5
{
  "book_id": 105,
  "title": "Cloud Native Spring in Action: With Spring Boot and Kubernetes",
  "rating": 5.0,
  "price": 88,
  "author": "Thomas Vitale",
  "pub_date": "2022-11-20",
  "publisher": "Manning"
}

GET /books/_mapping
```

## 更新现有字段？

```json
// NOT OK
PUT /books/_mapping
{
  "properties": {
    "book_id": {
      "type": "keyword"
    }
  }
}

// OK
PUT /books/_mapping
{
  "properties": {
    "publisher": {
      "type": "keyword",
      "ignore_above": 256
    }
  }
}
```

## Reindex

```json
PUT /books_new
{
  "mappings": {
    "properties": {
      "book_id": {
        "type": "keyword"
      },
      "title": {
        "type": "text"
      },
      "rating": {
        "type": "float"
      },
      "price": {
        "type": "integer"
      },
      "author": {
        "type": "text"
      },
      "pub_date": {
        "type": "date"
      },
      "publisher": {
        "type": "keyword"
      }
    }
  }
}

GET books_new/_mapping

POST /_reindex
{
  "source": {
    "index": "books"
  },
  "dest": {
    "index": "books_new"
  }
}

GET books_new/_search
{
  "query": {
    "match_all": {}
  }
}
```

## Reindex + Source 字段类型修改

```json
POST books_new/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}


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
      if (ctx._source.book_id != null) {
        ctx._source.book_id = ctx._source.book_id.toString();
      }
    """
  }
}

GET books_new/_search
{
  "query": {
    "match_all": {}
  }
}

```
