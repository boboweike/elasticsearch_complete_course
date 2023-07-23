Flattened(扁平数据类型)
===

* 对象中的字段值以`keyword`方式处理，不经过文本分析
  * 提升文本索引性能
  * 适用于对象字段比较多且不固定场景(无法提前显式定义mapping)
* 可以对整个字段进行查询，也可以对子字段进行查询

## 样例

```json
GET /_cat/indices?v

PUT consultations/_doc/1
{
  "patient_name": "John Doe",
  "doctor_notes": {
    "temperature": 103,
    "symptoms": ["chills", "fever", "headache"],
    "history": "none",
    "medication": ["Antibiotics", "Paracetamol"]
  }
}

GET consultations

DELETE consultations

PUT consultations
{
  "mappings": {
    "properties": {
      "patient_name": {
        "type": "text"
      },
      "doctor_notes": {
        "type": "flattened"
      }
    }
  }
}

GET consultations/_search
{
  "query": {
    "match": {
      "doctor_notes": "Paracetamol"
    }
  }
}

GET consultations/_search
{
  "query": {
    "match": {
      "doctor_notes.symptoms": "chills"
    }
  }
}

GET consultations/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"doctor_notes": "headache"}},
        {"match": {"doctor_notes": "Antibiotics"}}
      ],
      "must_not": [
        {"match": {"doctor_notes": "diabetics"}}
      ]
    }
  }
}
```