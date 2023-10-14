# 高级索引操作 ～ rollover API

```json
GET /_cat/indices?v

DELETE logs_2023-000001
DELETE logs_2023-000002

# Rollover API
# Split -> 将现有数据分散到更多shards中去
# Rollover -> 到达某个时间或大小，创建新索引，数据进入新索引，适用于时间序列数据

PUT /logs_2023-000001

# 别名必须指向一个writable index
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "logs_2023-000001",
        "alias": "latest_logs_a",
        "is_write_index": true
      }
    }
  ]
}

# 1. 将当前index设为只读(只能查询)
# 2. 根据命名惯例创建一个新的index
# 3. 将别名指向新的index
POST latest_logs_a/_rollover

GET /logs_2023-000001
GET /logs_2023-000002

# 问题：
# 1. 当index的大小超过一定的阈值时，能够自动rollover一个index?
# 2. 是否能够每天rollover到一个新的index?

# > 下一个视频 ～ 索引生命周期管理(ILM)
```
