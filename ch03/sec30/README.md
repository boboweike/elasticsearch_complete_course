```json
GET /_cat/indices?v

PUT /custom_analyzer_test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_custom_analyzer": {
          "type": "custom",
          "char_filter": ["html_strip"],
          "tokenizer": "standard",
          "filter": ["lowercase", "stop", "stemmer"]
        }
      }
    }
  }
}

POST /custom_analyzer_test/_analyze
{
  "analyzer": "my_custom_analyzer", 
  "text": "<p>Two dogs are barking on a <i>flying squirrel<i><p>"
}

POST _analyze
{
  "text": "<p>Two dogs are barking on a <i>flying squirrel<i><p>",
  "char_filter": ["html_strip"]
}

POST _analyze
{
  "text": "<p>Two dogs are barking on a <i>flying squirrel<i><p>",
  "char_filter": ["html_strip"], 
  "tokenizer": "standard", 
  "filter": ["lowercase", "stop", "stemmer"]
}
```