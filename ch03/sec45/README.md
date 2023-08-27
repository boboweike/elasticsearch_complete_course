## 动态映射

```json
PUT /books/_doc/1
{
  "title": "Head First Java, 2nd Edition",
  "rating": 4.5,
  "in_stock": 120,
  "classic": true,
  "authors": ["Kathy Sieera", "Bert Bates"],
  "pub_date": "2005-03-15",
  "publisher": {
    "name": "OReilly Media",
    "email": "support@oreilly.com"
  }
}

GET /books/_mapping
```

## 静态+动态映射

```json
PUT /movies
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text"
      },
      "release_date": {
        "type": "date"
      }
    }
  }
}

POST /movies/_doc
{
  "title": "The Godfather",
  "release_date": "1972-03-14",
  "running_time": 177
}

GET /movies/_mapping
```

## match+正则表达式

```json

DELETE /books

PUT /books
{
  "mappings": {
    "dynamic_templates": [
      {
        "author_tpl": {
          "match_mapping_type": "string",
          "match": "^[a-zA-Z]+_author$",
          "match_pattern": "regex",
          "mapping": {
            "type": "keyword"
          }
        }
      }
    ],
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
  "first_author": "Kathy Sieera",
  "second_author": "Bert Bates"
}

GET /books/_mapping
```

## path_match/path_unmatch 参数

```json
DELETE /books

PUT /books
{
  "mappings": {
    "dynamic_templates": [
      {
        "author_name_tpl": {
          "match_mapping_type": "string",
          "path_match": "author.name.*",
          "mapping": {
            "type": "keyword"
          }
        }
      }
    ],
    "properties": {
      "title": {
        "type": "text"
      }
    }
  }
}

POST /books/_doc
{
  "title": "A test book",
  "author": {
    "name": {
      "first_name": "Burnside",
      "middle_name": "Bartholomew",
      "last_name": "Stanley"
    }
  }
}

GET /books/_mapping
```
