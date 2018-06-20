---
title: "语法"
date: 2018-06-20T09:00:04+08:00
weight: 12
menu: main
---

### 过滤 (WHERE)

```
GET /数据库/模式/表?字段=$eq.值
```

查询操作符:

| 名称 | 说明 |
|-------|-------------|
| $eq | = 等于|
| $gt | > 大于|
| $gte | >= 大于等于|
| $lt | < 小于|
| $lte | <= 小于等于|
| $ne | <> 不等于|
| $in | in 匹配多值|
| $nin | not in 不匹配多值|
| $null | null 空值|
| $notnull | not null 不为空值|
| $true | true 真值|
| $nottrue | not true 不为真值|
| $false | false 假值|
| $notfalse | false 不为假值|
| $like | like 搜索某种模式|


### 过滤 (WHERE) with JSONb field

```
http://127.0.0.1:8000/数据库/模式/表?字段->>JSON字段:jsonb=值 (过滤)
```

### 查询(Select) - GET

```
http://127.0.0.1:8000/databases (返回所有表)
http://127.0.0.1:8000/databases?_count=* (返回所有表计数)
http://127.0.0.1:8000/databases?_renderer=xml (指定返回渲染格式,默认JSON)
http://127.0.0.1:8000/schemas (返回所有模式)
http://127.0.0.1:8000/schemas?_count=* (返回所有模式计数)
http://127.0.0.1:8000/schemas?_renderer=xml (指定返回渲染格式,默认JSON)
http://127.0.0.1:8000/tables (返回所有表)
http://127.0.0.1:8000/tables?_renderer=xml (指定返回渲染格式,默认JSON)
http://127.0.0.1:8000/数据库/模式 (返回指定表及模式下面的所有表)
http://127.0.0.1:8000/数据库/模式?_renderer=xml (指定返回渲染格式,默认JSON)
http://127.0.0.1:8000/数据库/模式/表 (返回指定表的所有记录)
http://127.0.0.1:8000/数据库/模式/表?_select=字段 (返回表的指定列的所有记录)
http://127.0.0.1:8000/数据库/模式/表?_select=多个字段[array id] (返回指定多个列的所有记录)

http://127.0.0.1:8000/数据库/模式/表?_select=* (返回指定表的所有记录)
http://127.0.0.1:8000/数据库/模式/表?_count=* (count(*)计数)
http://127.0.0.1:8000/数据库/模式/表?_count=字段 (返回指定字段计数)
http://127.0.0.1:8000/数据库/模式/表?_page=2&_page_size=10 (分页, pagination, page_size默认值10)
http://127.0.0.1:8000/数据库/模式/表?字段=值 (指定字段值)
http://127.0.0.1:8000/数据库/模式/表?_renderer=xml (指定返回渲染格式,默认JSON)


视图操作
http://127.0.0.1:8000/数据库/模式/视图?_select=字段 (返回视图指定列的所有记录)
http://127.0.0.1:8000/数据库/模式/视图?_select=* (返回指定视图的所有记录)
http://127.0.0.1:8000/数据库/模式/视图?_count=* (count(*)计数)
http://127.0.0.1:8000/数据库/模式/视图?_count=字段 (返回指定字段计数)
http://127.0.0.1:8000/数据库/模式/视图?_page=2&_page_size=10 (分页, pagination, page_size默认值10)
http://127.0.0.1:8000/数据库/模式/视图?字段=值 (指定字段值)
http://127.0.0.1:8000/数据库/模式/视图?_renderer=xml (JSON by default)

```

### 插入(Insert) - POST

```
http://127.0.0.1:8000/数据库/模式/表
```

JSON 数据:
```
{
    "字段1": "string value",
    "字段2": 1234567890
}
```

### 更新(Update) - PATCH/PUT

指定字段值更新, 示例:

```
http://127.0.0.1:8000/数据库/模式/表?字段=xyz
```

JSON DATA:
```
{
    "字段1": "string value",
    "字段2": 1234567890,
    "字段数组": ["value 1","value 2"]
}
```
### 删除(Delete) - DELETE

指定字段值删除, 示例:

```
http://127.0.0.1:8000/数据库/模式/表?字段=xyz
```

## 连接(JOIN)

```
/数据库/模式/表?_join=类型:表2:表1.字段:操作符:表2.字段
```
参数说明:

1. 类型 (INNER, LEFT, RIGHT, OUTER)
2. 表2
3. 表1.字段
4. 操作符 ($eq, $lt, $gt, $lte, $gte)
5. 表2.字段

示例:
```
/数据库/模式/friends?_join=inner:users:friends.userid:$eq:users.id
```

```
select * from friends inner join users
where friends.userid = users.id
```

## 操作符

| 名称 | 说明 |
|-------|-------------|
| $eq | = 等于|
| $gt | > 大于|
| $gte | >= 大于等于|
| $lt | < 小于|
| $lte | <= 小于等于|
| $ne | <> 不等于|
| $in | in 匹配多值|
| $nin | not in 不匹配多值|

## 去重(DISTINCT)

使用语法 `_distinct=true`, 启用去重 *DISTINCT*

示例:
```
    GET /数据库/模式/表/?_distinct=true
```

## 排序(ORDER BY)

通过*GET*请求方法中附加参数`_order`并指定字段(支持多个), 启用排序 *ORDER BY*. 如果需要降序排序, 可以指定前缀`-`. 如果需要指定多个字段排序, 可以用逗号`,`分割.

Examples:

### 升序(ASC)
    GET /DATABASE/SCHEMA/TABLE/?_order=fieldname

### 降序(DESC)
    GET /DATABASE/SCHEMA/TABLE/?_order=-fieldname

### 多字段排序
    GET /DATABASE/SCHEMA/TABLE/?_order=fieldname01,-fieldname02,fieldname03

## 分组(GROUP BY)

支持的分组函数:

| 名称 | 语法| 说明 |
| ------- | ------------- | ------------- |
| SUM | sum:field | 总数 |
| AVG | avg:field | 平均值 |
| MAX | max:field | 最大值 |
| MIN | min:field | 最小值 |
| MEDIAN | median:field | 中位值 |
| STDDEV | stddev:field | 标准差 | 
| VARIANCE | variance:field | 差值 |

### 示例:
	GET /数据库/模式/表/?_select=字段名00,字段名01&_groupby=字段名01

#### 分钟函数
	GET /数据库/模式/表/?_select=字段名00,sum:字段名01&_groupby=字段名01

#### 聚合限定(Having)
分组**Group By** 的聚合结果限定 **Having**:

	_groupby=字段名->>having:分组函数:字段名:条件:值-条件

示例:

	GET /数据库/模式/表/?_select=字段名00,sum:字段名01&_groupby=字段名01->>having:sum:字段名01:$gt:500
