响应裁剪
===
响应可能包含很多数据(例如source包含300个字段)
* 用户只关心部分数据
* 可能包含敏感字段
* 节省网络带宽

## 1.只获取source文档信息
```json
GET /books/_doc/1

GET /books/_source/1
```

## 2. 只获取元数据信息
```json
GET /books/_doc/1?_source=false
```

## 3. 只包含source文档的某些字段
```json
GET /books/_doc/2?_source_includes=title,Author

GET /books/_source/2?_source_includes=title,Author
```

## 4. 不包含source文档的某些字段
```json
GET /books/_source/3?_source_excludes=inStock,Rating
```

## 5. 包含和不包含混合使用
```json
PUT books/_doc/8
{
    "title" : "Core Java, 12th",
    "author" : "Cay Horstmann",
    "rating_jd" : 4.5,
    "rating_dangdang": 4.6,
    "rating_tmall": 4.5,
    "price" : 67,
    "inStock" : 150
}

GET books/_source/8?_source_includes=rating*&_source_excludes=rating_tmall
```
