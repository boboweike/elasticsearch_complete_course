对象(object)数据类型
===

## 对象数据类型

```json
PUT /emails
{
  "mappings": {
    "properties": {
      "to": {
        "type": "keyword"
      },
      "subject": {
        "type": "text"
      },
      "attachments": {
        "properties": {
          "filename": {
            "type": "keyword"
          },
          "filetype": {
            "type": "keyword"
          }
        }
      }
    }
  }
}

GET /emails/_mapping

PUT emails/_doc/1
{
  "to": "test01@boboweike.cn",
  "subject": "Testing Object type",
  "attachments": {
    "filename": "file1.txt",
    "filetype": "confidential"
  }
}

GET emails/_search
{
  "query": {
    "term": {
      "attachments.filename": "file1.txt"
    }
  }
}
```

## 对象数组

```json
PUT emails/_doc/2
{
  "to": "test02@boboweike.cn",
  "subject": "Multi attachments test",
  "attachments": [{
    "filename": "file2.txt",
    "filetype": "confidential"
  },{
    "filename": "file3.txt",
    "filetype": "private"
  }]
}

{
  "to": "test02@boboweike.cn",
  "subject": "Multi attachments test",
  "attachments.filename": ["file2.txt", "file3.txt"],
  "attachments.filetype": ["confidential", "private"]
}

GET emails/_search
{
  "query": {
    "bool": {
      "must": [
        {"term": { "attachments.filename": "file2.txt"}},
        {"term": { "attachments.filetype": "private"}}]
    }
  }
}

```

## 嵌套(nested)数据类型

```json

PUT /emails_nested
{
  "mappings": {
    "properties": {
      "to": {
        "type": "keyword"
      },
      "subject": {
        "type": "text"
      },
      "attachments": {
        "type": "nested",
        "properties": {
          "filename": {
            "type": "keyword"
          },
          "filetype": {
            "type": "keyword"
          }
        }
      }
    }
  }
}

PUT emails_nested/_doc/1
{
  "to": "test02@boboweike.cn",
  "subject": "Multi attachments test",
  "attachments": [{
    "filename": "file1.txt",
    "filetype": "confidential"
  },{
    "filename": "file2.txt",
    "filetype": "private"
  }]
}

GET emails_nested/_search
{
  "query": {
    "nested": {
      "path": "attachments",
      "query": {
        "bool": {
          "must": [
            {"term": { "attachments.filename": "file1.txt"}},
            {"term": { "attachments.filetype": "private"}}]
        }
      }
    }
  }
}

GET emails_nested/_search
{
  "query": {
    "nested": {
      "path": "attachements",
      "query": {
        "bool": {
          "must": [
            {"term": { "attachements.filename": "file2.txt"}},
            {"term": { "attachements.filetype": "private"}}]
        }
      }
    }
  }
}

```