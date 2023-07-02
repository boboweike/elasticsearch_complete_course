```json

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
  "text": "<p>Two dogs are barking on a <i>flying squirrel</i>",
  "analyzer": "my_custom_analyzer"
}

POST _analyze
{
  "text": "<p>Two dogs are barking on a <i>flying squirrel</i>",
  "char_filter": ["html_strip"]
}

```