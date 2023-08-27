## 配置 dynamic mapping

### dynamic = false

```json
DELETE /books

PUT /books
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text"
      },
      "pub_date": {
        "type": "date"
      }
    }
  }
}

PUT /books
{
  "mappings": {
    "dynamic": false,
    "properties": {
      "title": {
        "type": "text"
      },
      "pub_date": {
        "type": "date"
      }
    }
  }
}

POST /books/_doc
{
  "title": "Head First Java, 2nd Edition",
  "authors": ["Kathy Sieera", "Bert Bates"],
  "pub_date": "2005-03-15"
}

GET /books/_search

GET /books/_search
{
  "query": {
    "match": {
      "title": "java"
    }
  }
}

GET /books/_search
{
  "query": {
    "match": {
      "director": "bert"
    }
  }
}
```

### dynamic = "strict"

DELETE /books

```json
PUT /books
{
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "title": {
        "type": "text"
      },
      "pub_date": {
        "type": "date"
      }
    }
  }
}

POST /books/_doc
{
  "title": "Head First Java, 2nd Edition",
  "authors": ["Kathy Sieera", "Bert Bates"],
  "pub_date": "2005-03-15"
}
```

## dynamic 配置继承

```json
DELETE /books

PUT /books
{
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "title": {
        "type": "text"
      },
      "pub_date": {
        "type": "date"
      },
      "publisher": {
        "dynamic": true,
        "properties": {
          "name": {
            "type": "text"
          }
        }
      }
    }
  }
}

POST /books/_doc
{
  "title": "Head First Java, 2nd Edition",
  "authors": ["Kathy Sieera", "Bert Bates"],
  "pub_date": "2005-03-15",
  "publisher": {
    "name": "OReilly Media",
    "email": "support@oreilly.com"
  }
}

POST /books/_doc
{
  "title": "Head First Java, 2nd Edition",
  "pub_date": "2005-03-15",
  "publisher": {
    "name": "OReilly Media",
    "email": "support@oreilly.com"
  }
}
```

### 数值检测 Numeric detection

```json
DELETE /books

PUT /books
{
  "mappings": {
    "numeric_detection": true,
    "properties": {
      "title": {
        "type": "text"
      },
      "pub_date": {
        "type": "date"
      }
    }
  }
}

POST /books/_doc
{
  "title": "Head First Java, 2nd Edition",
  "pub_date": "2005-03-15",
  "rating": "4.5",
  "in_stock": "120"
}

GET /books/_mapping
```

## 日期检测 Date detection

```json
// "strict_date_optional_time", "yyyy/MM/dd HH:mm:ss Z||yyyy/MM/dd Z"

DELETE /books

PUT /books
{
  "mappings": {
    "date_detection": false,
    "properties": {
      "title": {
        "type": "text"
      }
    }
  }
}

POST /books/_doc
{
  "title": "Head First Java, 2nd Edition",
  "pub_date": "2005-03-15"
}

GET /books/_mapping
```

### 自定义日期格式 Date format

```json
DELETE /books

PUT /books
{
  "mappings": {
    "dynamic_date_formats": ["MM/dd/yyyy"],
    "properties": {
      "title": {
        "type": "text"
      }
    }
  }
}

POST /books/_doc
{
  "title": "Head First Java, 2nd Edition",
  "pub_date": "03/15/2005"
}

GET /books/_mapping
```
