Analyze API演示
===

```json
POST _analyze
{
  "text": "3.Hot cup of ☕️ and a 🍿 is a Weird Combo :(!!️",
  "analyzer": "standard"
}

POST _analyze
{
  "text": "3.Hot cup of ☕️ and a 🍿 is a Weird Combo :(!!️"
}

POST _analyze
{
  "text": "3.Hot cup of ☕️ and a 🍿 is a Weird Combo :(!!️",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}

POST _analyze
{
  "text": "3.Hot cup of ☕️ and a 🍿 is a Weird Combo :(!!️",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase", "stop"]
}

POST _analyze
{
  "text": "Kubernetes in Action️",
  "analyzer": "keyword"
}

```


