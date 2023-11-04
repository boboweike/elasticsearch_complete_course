# 通过 URI + 参数方式进行查询

```sh
# GET|POST <your_index_name>/_search?q=<name:value> AND|OR <name:value>

# 1. 通过title查询电影

# 查询所有title中含有Godfather的电影
GET movies/_search?q=title:Godfather

# 查询title中满足多个条件的电影，隐含OR操作
GET movies/_search?q=title:Godfather Knight Shawshank

# 2. AND操作

# 通过title AND actor查询电影
GET movies/_search?q=title:Knight AND actors:Bale

# 3. 额外参数

GET movies/_search?q=title:Godfather actors:(Brando OR Pacino) rating:(>=9.0 AND <=9.5)&from=0&size=10&sort=rating&default_operator=AND&explain=true

```
