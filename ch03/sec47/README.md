# 动态模版(dynamic template)

## 配置整数映射

```json
DELETE /books

PUT /books
{
  "mappings": {
    "dynamic_templates": [
      {
        "integer_tpl": {
          "match_mapping_type": "long",
          "mapping": {
            "type": "integer"
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
  "in_stock": 128
}

GET /books/_mapping
```

## 配置字符串映射

```json
DELETE /books

PUT /books
{
  "mappings": {
    "dynamic_templates": [
      {
        "string_tpl": {
          "match_mapping_type": "string",
          "mapping": {
            "type": "text"
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
  "authors": ["Kathy Sieera", "Bert Bates"]
}

GET /books/_mapping
```

## match/unmatch 参数

```json
DELETE /books

PUT /books
{
  "mappings": {
    "dynamic_templates": [
      {
        "string_keyword_tpl": {
          "match_mapping_type": "string",
          "match": "*_keyword",
          "mapping": {
            "type": "keyword"
          }
        }
      },
      {
        "strings_text_tpl": {
          "match_mapping_type": "string",
          "unmatch": "*_keyword",
          "mapping": {
            "type": "text"
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
  "description": "A book description",
  "isbn_keyword": "ABC_XYZ_666"
}

GET /books/_mapping
```

## path_match & path_unmarch 参数

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
