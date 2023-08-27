# Mapping 最佳实践

## 1. 生产环境使用显式 Mapping

- 动态 Mapping 适用于开发阶段
- 显式 Mapping 可以进一步优化参数节省磁盘空间(对于大索引)
- dynamic 设为`strict`，而不是 `false`
  - 避免不可预知的问题(unexpected surprise)
  - `true`支持动态映射
  - `false`忽略动态字段
  - `strict`拒绝不符合 mapping schema 的文档

## 2. 文本(text)字段避免同时 Mapping 为 text + keyword 类型

- 通常只需要一种，多一个字段类型就会创建额外的索引结构，需要额外存储空间
- 主要场景是全文搜索 -> text 类型
- 主要场景是聚合/排序/精准匹配/过滤 -> keyword 类型

## 3. 禁用 coercion

- 要求客户端输入严格匹配的类型，避免不可预知的问题(unexpected surprise)

## 4. 数值类型优化

- 节省磁盘存储
  - long > integer
  - double > float

## 5. Mapping 参数优化

- 字段不需要排序/聚合/脚本(script)访问，将 doc_values 设为 false
- 字段不需要相关性打分，将 norms 设为 false
- 字段不需要查询/过滤, 将 index 设为 false
  - 仍然可以做聚合，适用于时间序列(time series)数据
- 大索引适用(>1 百万)
- 尽量事前优化，后期优化需要 Reindex

## 下一章预览

### 第 4 章，索引操作

- 创建/读取/删除/关闭/打开索引
- 监控和管理索引
- 高级操作, Split/Shrink/Index alias
- 索引生命周期管理(ILM)
