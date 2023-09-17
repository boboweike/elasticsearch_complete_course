# 组合索引模版

```json
GET /_cat/indices?v

DELETE books_java
DELETE _index_template/composed_books_template
DELETE _component_template/qa_settings_component_template
DELETE _component_template/qa_mappings_component_template

# 创建索引模版
PUT _index_template/books_template
{
  "index_patterns": ["books_*"],
  "priority": 1,
  "template": {
    "mappings": {
      "properties": {
        "pub_date": {
          "type": "date"
        },
        "publisher": {
          "type": "keyword"
        }
      }
    }
  }
}

GET _index_template/books_template

DELETE _index_template/books_template

# 创建组件模版
POST _component_template/qa_settings_component_template
{
  "template": {
    "settings": {
      "number_of_shards": 3,
      "number_of_replicas": 1
    }
  }
}

GET _component_template/qa_settings_component_template

POST _component_template/qa_mappings_component_template
{
  "template": {
    "mappings": {
      "properties": {
        "pub_date": {
          "type": "date"
        },
        "publisher": {
          "type": "keyword"
        }
      }
    }
  }
}

GET _component_template/qa_mappings_component_template

POST _index_template/composed_qa_books_template
{
  "index_patterns": ["books_*"],
  "priority": 20,
  "composed_of": ["qa_settings_component_template", "qa_mappings_component_template"]
}

GET _index_template/composed_qa_books_template

PUT books_java/_doc/1
{
  "title": "Head First Java, 2nd Edition",
  "pub_date": "2005-03-15",
  "publisher": "Oreilly_Media"
}

GET books_java
```
