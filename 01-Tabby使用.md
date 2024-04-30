# 01-Tabby使用

## 命令行使用方式
```bash
# 分析数据
java -Xmx16g -jar tabby.jar

# 加载数据到neo4j
java -jar tabby-vul-finder.jar load output/dev

# 分析数据可疑链路
java -jar tabby-vul-finder.jar query project_name
```
## 配置文件
数据库配置
```properties
# for docker
tabby.cache.isDockerImportPath            = false

# db settings
tabby.neo4j.username                      = neo4j
tabby.neo4j.password                      = password
tabby.neo4j.url                           = bolt://127.0.0.1:7687
```
软件配置
若进行分析需每次重新配置settings.properties下的tabby.build.target目标，可以为jar，可以为目录
```properties
# need to modify
# 给定待分析目标，可以是文件夹也可以是单个文件, 指定待分析文件或文件夹。
tabby.build.target                        = cases/jimureport-spring-boot-starter-1.6.1.jar
# 不需要全量分析的依赖文件目录，加快分析速度, 指定需要额外加入的依赖文件，防止缺失部分节点，比如web模式下最好加入servlet相关的依赖。
tabby.build.libraries                     = libs
 # 分析类型 web 或 gadget，web模式会剔除常见jar包的全量分析，gadget模式会对target目录下的文件进行全量分析,分析过程会分析所有待分析文件。web：分析过程会剔除commonJars.json文件内的公共Jar文件，加速分析。
tabby.build.mode                          = gadget
tabby.output.directory                    = ./output/dev

# debug
tabby.debug.details                       = false
tabby.debug.print.current.methods         = true

# jdk settings
# 当需要指定特定 JDK 版本时，tabby.build.useSettingJRE改为true
# 同时配置好tabby.build.javaHome，并根据指定的JDK版本是否大于9，来配置tabby.build.isJRE9Module。
# 如果修改了tabby.build.javaHome内容，则需要同时删除jre_libs目录，不然修改将不会生效。
tabby.build.useSettingJRE                 = false
tabby.build.isJRE9Module                  = true
tabby.build.javaHome                      = /Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home
# 分析过程是否加入基础的2个jdk依赖,将会加入几个JDK的基础依赖一起分析。
tabby.build.isJDKProcess                  = false
# 分析过程是否加入全量的jdk依赖,将会加入所有的JDK依赖一起分析。
tabby.build.withAllJDK                    = false
# 分析过程是否仅分析jdk依赖，不会去分析target目录下的文件,将只分析所有JDK依赖，前面配置的target将不会被分析。
tabby.build.isJDKOnly                     = false

# dealing fatjar
tabby.build.checkFatJar                   = true

# pointed-to analysis
# 默认启用污点分析来生成对应的代码属性图，但该模式下可能存在分析不准确的情况
# 如果改成true，则会生成全量函数调用图，可以使用apoc插件来进行结果检索。
tabby.build.isFullCallGraphCreate         = false
tabby.build.thread.timeout                = 2
tabby.build.method.timeout                = 5
tabby.build.isNeedToCreateIgnoreList      = false
tabby.build.timeout.forceStop             = false
tabby.build.isNeedToDealNewAddedMethod    = true
```
## neo4j数据库配置方式
要选择版本为5.3.0的版本
![image.png](https://cdn.nlark.com/yuque/0/2024/png/1348791/1714111685867-d0f72533-3e67-4017-a3f8-1a16df514688.png#averageHue=%23fdfcfc&clientId=u2002b408-7f7c-4&from=paste&height=372&id=u014971ef&originHeight=372&originWidth=515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13404&status=done&style=none&taskId=u1aa32556-4c7c-4861-93ee-b0b3c3a2dcd&title=&width=515)
