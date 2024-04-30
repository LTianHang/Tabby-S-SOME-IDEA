# 03-使用Tabby进行漏洞查询

## Tabby查询语句

Tabby的数据关系为Class节点与Method节点
[现有利用链覆盖](https://github.com/wh1t3p1g/tabby/wiki/%E7%8E%B0%E6%9C%89%E5%88%A9%E7%94%A8%E9%93%BE%E8%A6%86%E7%9B%96)

### Class节点
Class节点用于描述对象的基础信息，包含类名、注解等等
查看Class节点基础信息
```
MATCH (n:Class) RETURN n LIMIT 1
```
各Class节点的各属性解释
```json
{
  // 是否为接口实现类
	"HAS_INTERFACES": true,
  // 类的注解 例如 @Configuration(\"jimuReportConfiguration\")
  // 该类下存在三处注解
	"ANNOTATIONS": {
		"org.springframework.context.annotation.Configuration": {
			"value": [
				"jimuReportConfiguration"
			]
		},
		"org.springframework.context.annotation.PropertySource": {
			"value": [
				"classpath:config/default-config.properties"
			]
		},
		"org.springframework.context.annotation.ComponentScan": {
			"value": [
				"org.jeecg.modules.jmreport"
			]
		}
	},
  // 是否为struts的active执行器
	"IS_STRUTS_ACTION": false,
  // 是否为abstract抽象类
	"IS_ABSTRACT": false,
  // 类名称 包名+类名
	"NAME": "org.jeecg.modules.jmreport.config.init.JimuReportConfiguration",
  // 此类下的子类名称
	"CHILD_CLASSNAMES": "[]",
  // 是否有默认构造函数
	"HAS_DEFAULT_CONSTRUCTOR": true,
  // 是否具有父类
	"HAS_SUPER_CLASS": true,
  // 本类继承于哪个interface
	"INTERFACES": [
		"org.springframework.beans.factory.InitializingBean",
		"org.springframework.web.servlet.config.annotation.WebMvcConfigurer"
	],
  // 是否为接口类
	"IS_INTERFACE": false,
  // ID
	"ID": "a94bb0e7c86f820dc820a9dd6f097426",
  // 类的权限修饰符是否为PUBLIC
	"IS_PUBLIC": true,
  // 类是否可以被序列化
	"IS_SERIALIZABLE": false,
  // 类的超类 Object
	"SUPER_CLASS": "java.lang.Object"
}
```
类的常用属性节点
```json
NAME : 包含包名的类名
IS_INTERFACE : 描述当前类是否为接口类型
```
### Method节点
查看Method基础节点信息
```json
MATCH (n:Method) RETURN n LIMIT 1
```
```json
{
  // 该方法下存在的注解
	"ANNOTATIONS": "{}",
  // 函数名称
	"NAME": "equals",
  // 函数位置 可直接在idea中定位代码
	"NAME0": "java.lang.Double#equals",
  // 函数对象名称
	"CLASSNAME": "java.lang.Double",
  // 是否来自抽象类
	"IS_FROM_ABSTRACT_CLASS": false,
  // 是否为Source
	"IS_SOURCE": false,
  // 是否为SINK
	"IS_SINK": false,
  // 函数是否为public类型
	"IS_PUBLIC": true,
  // 是否忽略
	"IS_IGNORE": false,
  // 污染位置
	"POLLUTED_POSITION": "[]",
  // 是否为抽象方法
	"IS_ABSTRACT": false,
  // 函数签名
	"SIGNATURE": "<java.lang.Double: boolean equals(java.lang.Object)>",
  // 函数子签名 不包含所属对象名称
	"SUB_SIGNATURE": "boolean equals(java.lang.Object)",
  // 是否需要参数
	"HAS_PARAMETERS": true,
  // 需传入实际参数的个数
	"PARAMETER_SIZE": 1,
  // 是否为web端点 用于定义source函数
	"IS_ENDPOINT": false,
  // 描述函数是否为netty端点  常用于定义source函数
	"IS_NETTY_ENDPOINT": false,
  //函数修饰词
	"MODIFIERS": 1,
  // 是否为类默认构造方法
	"HAS_DEFAULT_CONSTRUCTOR": false,
  // 方法ID
	"ID": "65dd2958d7ce263efe72aa9aa03cf318",
  // 返回值类型
	"RETURN_TYPE": "boolean",
  // 漏洞类型 是否包含 file filewrite cide JNDI xxe ssrf serialize
	"VUL": "",
  // 是否为包含源
	"IS_CONTAINS_SOURCE": false,
  // 是否为静态函数类型
	"IS_STATIC": false,
  // 是否为可序列化的 同Class节点的属性功能一致
	"IS_SERIALIZABLE": false,
  // 是否为setter方法
	"IS_SETTER": false
  // 是否为getter方法
	"IS_GETTER": false,
  //
	"TYPE": ""
  // 
	"IS_ACTION_CONTAINS_SWAP": false,
  //
	"URL_PATH": "",
  // 
	"ACTIONS": "{}",
  // 
	"IS_CONTAINS_OUT_OF_MEM_OPTIONS": false,
}
```
### 代码数据库内的边与边
`copy` Tabby代码属性图使用教程内数据

```cypher
match(source:Method {IS_ENDPOINT: true})
match(source:Method {IS_NETTY_ENDPOINT: true})
```
```cypher
match(sink:Method {IS_SINK: true})
match(sink:Method {IS_SINK: true, VUL:"EXEC"})
```
```cypher
match(source:Method {IS_STATIC:true, IS_PUBLIC:true})
```
```cypher
match(source:Method {IS_GETTER:true})
match(source:Method {IS_SETTER:true})
```
#### HAS 边
HAS 边用于描述 Class 和 Method 节点的所属关系
```cypher
match (c:Class)-[:HAS]->(m:Method)
```
#### EXTENDS边、INTERFACE 边
EXTENDS 边用于描述父类子类之间的关系
INTERFACE 边用于描述对象实现的关系
```cypher
match (child:Class)-[:EXTENDS*]->(father:Class)
```
```cypher
match (child:Class)-[:EXTENDS|INTERFACE*]->(father:Class)
```
```cypher
match (m:Method)<-[:HAS]-(child:Class)-[:EXTENDS|INTERFACE*]->(father:Class)
```
#### CALL 边
CALL 边用于描述函数的调用关系
```cypher
match (caller:Method)-[:CALL]->(callee:Method)
```
#### ALIAS 边
ALIAS 边用于描述函数的所有具体实现
```cypher
match (m1:Method)-[:ALIAS*]->(m2:Method)
```
### 进行简单的Tabby查询
#### 查询方法下调用的方法
**例如 查询入口点URL_PATH为/xxx/xxx/xxx下的方法中调用的方法**

```cypher
match(m:Method {URL_PATH:"/xxx/xxx/xxx"})-[:CALL]->(m1:Method) return m,m1
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/1348791/1714460770120-1d523524-de6b-4d19-90bd-1e9660c88fcf.png#averageHue=%23f8fbfe&clientId=u93355250-ac3a-4&from=paste&height=467&id=u57c36571&originHeight=467&originWidth=733&originalType=binary&ratio=1&rotation=0&showTitle=false&size=55226&status=done&style=none&taskId=ud206c9f5-7c2b-4c6d-9484-0d07f50b5e8&title=&width=733)
#### **查找方法下调用的接口实现类**
**查找通过方法调用的接口以及接口实现类的方法**
```cypher
match(m:Method {URL_PATH:"/xxx/xxx/xxx"})
-[:CALL]->(m1:Method {IS_ABSTRACT:true})
-[:ALIAS]-(m2:Method)
return *
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/1348791/1714460458646-6777734f-9313-42fd-a358-f6dc8f13ae43.png#averageHue=%23f8fbfe&clientId=u3a4568c1-39bd-4&from=paste&height=436&id=u6f346b7b&originHeight=436&originWidth=819&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34813&status=done&style=none&taskId=ua78b7cd4-eb00-4c34-836d-0c7e95c1a5e&title=&width=819)
#### **通过区间进行查找**
**如果不确定是在多少层被调用的 在这里可以使用区间进行操作**

```cypher
match(m:Method {URL_PATH:"/xxx/xxx/xxx"})
-[:CALL]->(m1:Method {IS_ABSTRACT:true})
-[:ALIAS]-(m2:Method)
// 假设 方法在接下来的第一到十层被调用 且调用的方法为FreemarkerUtils类中的方法
-[:CALL*1..10]->(m3:Method {CLASSNAME:"com.test.freemarker.ssti.FreemarkerUtils"})
return *
```
这样的话不会显示具体路径,需要手动进行路径查找
![image.png](https://cdn.nlark.com/yuque/0/2024/png/1348791/1714460722312-a1a9c452-deb6-44a3-9637-b82976f0ae47.png#averageHue=%23f8fbfe&clientId=u93355250-ac3a-4&from=paste&height=426&id=ubd9711cf&originHeight=426&originWidth=735&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14605&status=done&style=none&taskId=u8e2e3d52-cc57-475d-925b-497ff662108&title=&width=735)
模糊尝试查找路径，上面条件为真，也就是在区间包含在1-10层中，那么可以构造下具体路径语句，或者将1-10的范围逐渐缩小，确定是在哪一层被调用的，例如 1-9，1-8...... 知道确定具体层级，确定具体层级后，进行精确脚本编写
例如：1-1查询不到数据，就代表存在于1-2中，因为1-1没数据，所以调用层级应该为2层

```cypher
match(m:Method {URL_PATH:"/xxx/xxx/xxx"})
-[:CALL]->(m1:Method {IS_ABSTRACT:true})
-[:ALIAS]-(m2:Method)
-[:CALL*1..2]->(m3:Method {CLASSNAME:"com.test.freemarker.ssti.FreemarkerUtils"})
return *
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/1348791/1714461354392-aeb47b34-722e-4fed-ba63-13fa3c47dc53.png#averageHue=%23f8fbfe&clientId=u93355250-ac3a-4&from=paste&height=454&id=u38ddbb39&originHeight=454&originWidth=829&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16952&status=done&style=none&taskId=u04bed497-df63-4982-a046-f19187dbcbd&title=&width=829)
构建具体路径代码
```cypher
match(m:Method {URL_PATH:"/xxx/xxx/xxx"})
-[:CALL]->(m1:Method {IS_ABSTRACT:true})
-[:ALIAS]-(m2:Method)
-[:CALL]->(m3:Method)
-[:CALL]->(m4:Method {CLASSNAME:"com.test.freemarker.ssti.FreemarkerUtils"})
return *
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/1348791/1714461207600-ee6e6106-ca36-4196-82d4-57395a94a5a4.png#averageHue=%23f8fbfe&clientId=u93355250-ac3a-4&from=paste&height=471&id=u4f6d0b67&originHeight=471&originWidth=942&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19351&status=done&style=none&taskId=u7e27ece5-8841-4ad4-8025-95505768092&title=&width=942)

