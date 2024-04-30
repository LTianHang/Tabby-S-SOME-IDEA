# 02-Neo4j数据库基础语法

[Neo4j 语法-CSDN博客](https://blog.csdn.net/fengxing_2/article/details/130606727)
[【Neo4j与知识图谱】Neo4j的常用语法与一个简单知识图谱构建示例_neo4j创建节点和关系-CSDN博客](https://blog.csdn.net/QH2107/article/details/129658674)
[Neo4j 语法_neo4j语法-CSDN博客](https://blog.csdn.net/fengxing_2/article/details/130606727)

## 基础语法
创建一个demo的语法

```cypher
create(:User {name:"user1"})
create(:User {name:"user2"})
create(:User {name:"user3"})
create(:User {name:"user4"})
create(:User {name:"user5"})

create(:Class {name:"class1"})
create(:Class {name:"class2"})
create(:Class {name:"class3"})
create(:Class {name:"class4"})
create(:Class {name:"class5"})
create(:Class {name:"class6"})
create(:Class {name:"class7"})
create(:Class {name:"class8"})
create(:Class {name:"class9"})
create(:Class {name:"class0"})


match(u:User {name:"user1"}),(c:Class {name:"class1"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class2"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class3"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class4"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class5"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class6"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class7"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class8"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class9"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user1"}),(c:Class {name:"class0"}) create(u)-[contain:contain]->(c)

match(u:User {name:"user2"}),(c:Class {name:"class1"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class2"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class3"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class4"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class5"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class6"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class7"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class8"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class9"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user2"}),(c:Class {name:"class0"}) create(u)-[contain:contain]->(c)

match(u:User {name:"user3"}),(c:Class {name:"class1"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class2"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class3"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class4"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class5"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class6"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class7"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class8"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class9"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user3"}),(c:Class {name:"class0"}) create(u)-[contain:contain]->(c)

match(u:User {name:"user4"}),(c:Class {name:"class1"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class2"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class3"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class4"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class5"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class6"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class7"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class8"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class9"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user4"}),(c:Class {name:"class0"}) create(u)-[contain:contain]->(c)

match(u:User {name:"user5"}),(c:Class {name:"class1"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class2"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class3"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class4"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class5"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class6"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class7"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class8"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class9"}) create(u)-[contain:contain]->(c)
match(u:User {name:"user5"}),(c:Class {name:"class0"}) create(u)-[contain:contain]->(c)
```



### 创建节点
```json
create(:User {name:"admin",age:30})
```
### 删除节点
```json
match (u:User {name:"admin"}) delete u
```
### 创建关系
区别 
	使用create语句 创建关系时 会生成单条数据
	使用match语句现行查询,查询完毕之后,再使用create就会使用当前数据进行关系生成
```json
match(u:User {name:"user1"}),(c:Class {name:"class1"})
create(u)-[:contain]->(c)
```
### 删除关系
```json
// 删除给定条件规则的关系
match(u:User {name:"user1"})-[contain:contain]->(c:Class {name:"class2"})
delete contain
// 删除全部名称为contain的关系
match(u)-[contain:contain]->(c)
delete contain
```
### 查询关系
```json
// 查询系统中所有的关系
match(u)-[contain:contain]->(c) return u,contain,c
// 查询条件为真的关系
match(u {name:"user1"})-[contain:contain]->(c) return u,contain,c
match(u:User {name:"user1"})-[contain:contain]->(c) return u,contain,c
```

