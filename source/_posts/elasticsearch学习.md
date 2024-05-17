---
title: elasticsearch学习
language: zh-CN
date: 2024-05-17 14:19:57
tags: 
- elasticsearch
- 搜索引擎
- 分词
- 分布式
---
### 0、概述
> ES是一个基于RESTful web接口并且构建在Apache Lucene之上的开源分布式搜索引擎。同时ES还是一个分布式文档数据库，其中每个字段均可被索引，而且每个字段的数据均可被搜索，能够横向扩展至数以百计的服务器存储以及处理PB级的数据。可以在极短的时间内存储、搜索和分析大量的数据。通常作为具有复杂搜索场景情况下的核心发动机。ES就是为高可用和可扩展而生的。
>一方面可以通过升级硬件来完成系统扩展，称为垂直或向上扩展（Vertical Scale/Scaling Up）。
另一方面，增加更多的服务器来完成系统扩展，称为水平扩展或者向外扩展（Horizontal Scale/Scaling Out）。
尽管ES能够利用更强劲的硬件，但是垂直扩展毕竟还是有它的极限。真正的可扩展性来自于水平扩展，通过向集群中添加更多的节点来分担负载，增加可靠性。
ES天生就是分布式的，它知道如何管理多个节点来完成扩展和实现高可用性。意味应用不需要做任何的改动。

### 1、elasticsearch安装部署
#### 1.1、**安装包下载**
- 官网的下载地址：[下载地址](https://www.elastic.co/cn/downloads/elasticsearch)

#### 1.2、**解压和运行**
##### 1.2.1、**解压安装包**
	- **elasticsearch目录结构**![[Pasted image 20240514102525.png]]
	- **目录名词解释**
>		bin：可执行文件在里面，运行es的命令就在这个里面，启动es也是在bin目录下启动
>		config：配置文件目录
>		JDK：java环境
>		lib：依赖的jar，类库
>		logs：日志文件
>		modules：es相关的模块
>		plugins：可以自己开发的插件 我们下载的插件也是放在里面

##### 1.2.2、 **修改核心配置文件 elasticearch.yml**
	- 最新版本（8.13.3）可以直接进入bin目录直接运行elasticsaearch.bat  初次运行中会自动配置相关参数，默认为https
```yml
# ======================== Elasticsearch Configuration =========================
#
# NOTE: Elasticsearch comes with reasonable defaults for most settings.
#       Before you set out to tweak and tune the configuration, make sure you
#       understand what are you trying to accomplish and the consequences.
#
# The primary way of configuring a node is via this file. This template lists
# the most important settings you may want to configure for a production cluster.
#
# Please consult the documentation for further information on configuration options:
# https://www.elastic.co/guide/en/elasticsearch/reference/index.html
#
# ---------------------------------- Cluster集群配置 -----------------------------------
#
# Use a descriptive name for your cluster:
#
#cluster.name: my-application
#
# ------------------------------------ Node 节点配置 ------------------------------------
#
# Use a descriptive name for the node:
#
#node.name: node-1
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
#
# ----------------------------------- Paths 路径配置------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
#path.data: /path/to/data
#
# Path to log files:
#
#path.logs: /path/to/logs
#
# ----------------------------------- Memory 内存配置-----------------------------------
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network 网络配置-----------------------------------
#
# By default Elasticsearch is only accessible on localhost. Set a different
# address here to expose this node on the network:
#
#network.host: 192.168.0.1
#
# By default Elasticsearch listens for HTTP traffic on the first free port it
# finds starting at 9200. Set a specific HTTP port here:
#
#http.port: 9200
#
# For more information, consult the network module documentation.
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
#discovery.seed_hosts: ["host1", "host2"]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
#cluster.initial_master_nodes: ["node-1", "node-2"]
#
# For more information, consult the discovery and cluster formation module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Allow wildcard deletion of indices:
#
#action.destructive_requires_name: false

#----------------------- BEGIN SECURITY AUTO CONFIGURATION -----------------------
#
# The following settings, TLS certificates, and keys have been automatically      
# generated to configure Elasticsearch security features on 07-05-2024 01:47:34
#
# --------------------------------------------------------------------------------

# Enable security features
xpack.security.enabled: true

xpack.security.enrollment.enabled: true

# Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
xpack.security.http.ssl:
  enabled: false
  keystore.path: certs/http.p12

# Enable encryption and mutual authentication between cluster nodes
xpack.security.transport.ssl:
  enabled: true
  verification_mode: certificate
  keystore.path: certs/transport.p12
  truststore.path: certs/transport.p12
# Create a new cluster with the current node only
# Additional nodes can still join the cluster later
cluster.initial_master_nodes: ["DESKTOP-5AI9SIK"]

# Allow HTTP API connections from anywhere
# Connections are encrypted and require user authentication
# 配置可以以ip访问服务
http.host: 0.0.0.0

# Allow other nodes to join the cluster from anywhere
# Connections are encrypted and mutually authenticated
#transport.host: 0.0.0.0

#----------------------- END SECURITY AUTO CONFIGURATION -------------------------
```

##### 1.2.3、 **修改 jvm 参数(jvm.options)**
	- 最新版（8.13.3）默认设置为物理机内存的50%，7.X版本（7.6.2)默认配置为1G
```options
-Xms512m  
-Xmx512m
```

##### 1.2.4、 **添加用户（ linux平台）**
```bash
# 1. 创建elastic用户组及elastic用户：
groupadd elastic  
useradd elastic -g elastic  
passwd elastic
# 接下来会输入两次密码  
# new password  
# retype passwd

# 2. 切换到elastic用户再启动  
su elastic
```

##### 1.2.5、 **系统性能优化（ linux平台）**
```bash
cat /etc/security/limits.conf
* soft nofile 655360
* hard nofile 655360
 
cat /etc/sysctl.conf
vm.max_map_count = 655360

#保存退出后使用下面命令使其生效

sysctl -p
```

##### 1.2.6、 **启动elasticsearch**
	- 进入`bin`目录
	- `./elasticsearch `或`./elasticsearch.bat`
	- 如后台运行，可执行`./elasticsearch -d`或`./elasticsearch.bat -d`

#### 1.3、集群部署
- 集群部署参数配置（elasticsearch.yml）
- master数据节点
```yml
# ======================== Elasticsearch Configuration =========================
#
# NOTE: Elasticsearch comes with reasonable defaults for most settings.
#       Before you set out to tweak and tune the configuration, make sure you
#       understand what are you trying to accomplish and the consequences.
#
# The primary way of configuring a node is via this file. This template lists
# the most important settings you may want to configure for a production cluster.
#
# Please consult the documentation for further information on configuration options:
# https://www.elastic.co/guide/en/elasticsearch/reference/index.html
#
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster:
#
cluster.name: my-application
#
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
node.name: node-1
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
#
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
#path.data: /path/to/data
#
# Path to log files:
#
#path.logs: /path/to/logs
#
# ----------------------------------- Memory -----------------------------------
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network -----------------------------------
#
# By default Elasticsearch is only accessible on localhost. Set a different
# address here to expose this node on the network:
#
network.host: 192.168.0.152
#
# By default Elasticsearch listens for HTTP traffic on the first free port it
# finds starting at 9200. Set a specific HTTP port here:
#
http.port: 9201
#
# For more information, consult the network module documentation.
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
discovery.seed_hosts: ["192.168.0.152:9301", "192.168.0.152:9302", "192.168.0.152:9303", "192.168.0.152:9304", "192.168.0.152:9305"]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
cluster.initial_master_nodes: ["node-1", "node-2", "node-3"]
#
# For more information, consult the discovery and cluster formation module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Allow wildcard deletion of indices:
#
#action.destructive_requires_name: false

#----------------------- BEGIN SECURITY AUTO CONFIGURATION -----------------------
#
# The following settings, TLS certificates, and keys have been automatically      
# generated to configure Elasticsearch security features on 07-05-2024 01:47:34
#
# --------------------------------------------------------------------------------

# Enable security features
xpack.security.enabled: true

xpack.security.enrollment.enabled: true

# Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
xpack.security.http.ssl:
  enabled: false
  keystore.path: certs/http.p12

# Enable encryption and mutual authentication between cluster nodes
xpack.security.transport.ssl:
  enabled: true
  verification_mode: certificate
  keystore.path: certs/transport.p12
  truststore.path: certs/transport.p12
# Create a new cluster with the current node only
# Additional nodes can still join the cluster later
# cluster.initial_master_nodes: ["DESKTOP-5AI9SIK"]

# Allow HTTP API connections from anywhere
# Connections are encrypted and require user authentication
http.host: 0.0.0.0

# Allow other nodes to join the cluster from anywhere
# Connections are encrypted and mutually authenticated
#transport.host: 0.0.0.0

transport.port: 9301

node.roles: [master, data]

gateway.recover_after_data_nodes: 3

#----------------------- END SECURITY AUTO CONFIGURATION -------------------------

```

- 协调节点
```yml
# ======================== Elasticsearch Configuration =========================
#
# NOTE: Elasticsearch comes with reasonable defaults for most settings.
#       Before you set out to tweak and tune the configuration, make sure you
#       understand what are you trying to accomplish and the consequences.
#
# The primary way of configuring a node is via this file. This template lists
# the most important settings you may want to configure for a production cluster.
#
# Please consult the documentation for further information on configuration options:
# https://www.elastic.co/guide/en/elasticsearch/reference/index.html
#
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster:
#
cluster.name: my-application
#
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
node.name: node-4
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
#
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
#path.data: /path/to/data
#
# Path to log files:
#
#path.logs: /path/to/logs
#
# ----------------------------------- Memory -----------------------------------
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network -----------------------------------
#
# By default Elasticsearch is only accessible on localhost. Set a different
# address here to expose this node on the network:
#
network.host: 192.168.0.152
#
# By default Elasticsearch listens for HTTP traffic on the first free port it
# finds starting at 9200. Set a specific HTTP port here:
#
http.port: 9204
#
# For more information, consult the network module documentation.
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
discovery.seed_hosts: ["192.168.0.152:9301", "192.168.0.152:9302", "192.168.0.152:9303", "192.168.0.152:9304", "192.168.0.152:9305"]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
cluster.initial_master_nodes: ["node-1", "node-2", "node-3"]
#
# For more information, consult the discovery and cluster formation module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Allow wildcard deletion of indices:
#
#action.destructive_requires_name: false

#----------------------- BEGIN SECURITY AUTO CONFIGURATION -----------------------
#
# The following settings, TLS certificates, and keys have been automatically      
# generated to configure Elasticsearch security features on 07-05-2024 01:47:34
#
# --------------------------------------------------------------------------------

# Enable security features
xpack.security.enabled: true

xpack.security.enrollment.enabled: true

# Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
xpack.security.http.ssl:
  enabled: false
  keystore.path: certs/http.p12

# Enable encryption and mutual authentication between cluster nodes
xpack.security.transport.ssl:
  enabled: true
  verification_mode: certificate
  keystore.path: certs/transport.p12
  truststore.path: certs/transport.p12
# Create a new cluster with the current node only
# Additional nodes can still join the cluster later
# cluster.initial_master_nodes: ["DESKTOP-5AI9SIK"]

# Allow HTTP API connections from anywhere
# Connections are encrypted and require user authentication
http.host: 0.0.0.0

# Allow other nodes to join the cluster from anywhere
# Connections are encrypted and mutually authenticated
#transport.host: 0.0.0.0

transport.port: 9304

node.roles: []

gateway.recover_after_data_nodes: 3

#----------------------- END SECURITY AUTO CONFIGURATION -------------------------

```
- 节点角色说明：
	- 主节点是管理集群状态的(对所有资源要求都不高)
	- 协调节点是接受客户端请求并分发到相应数据节点的，并合并搜索结果(对网络资源要求高，对CPU和内存也有一定的要求)
	- 数据节点是读写数据的(对内存、CPU和IO要求高)
	- 注：一般来说，在生产环境中，为了节省资源一个节点一般都是有多个角色，很少将节点作为单一角色来使用
- 最小的集群：将一个节点当作所有角色来使用(每个节点既是数据节点，也是主节点和协调节点)
- 初等规模：分成主节点和其他节点(数据节点也是协调节点)；分成协调节点和其他节点(数据节点也是主节点);
- 中高等规模：将数据节点、主节点和协调节点分开

### 2、安装分词器
#### 2.1、**分词器安装**
```bat
 .\elasticsearch-plugin.bat install https://get.infini.cloud/elasticsearch/analysis-ik/8.13.3
```
#### 2.2、 分析器测试
```
GET /_analyze
{
  "analyzer": "simple",
  "text": ["我是张三"]
}

GET /_analyze
{
  "analyzer": "whitespace",
  "text": ["我 是 张三"]
}

GET /_analyze
{
  "analyzer": "ik_smart",
  "text": ["我是张三"]
}

GET /_analyze
{
  "analyzer": "ik_max_word",
  "text": ["我是张三"]
}
```
- 新建词库或更新词库
- 新建词库
	- 新建.dic文件在分词器插件的配置目录（plugins/xxx/conf  或conf/xxx/）
	- 修改`IKAnalyzer.cfg.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">XXX.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords"></entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>

```
- 更新词库，将新增词添加到.dic的词库文件中

### 3、 **索引的基本操作**
#### 3.1、新增或更新索引
```
POST /product/_doc/1
{
  "name": "测试",
  "type": "1"
}
```

#### 3.2、批量新增索引
```
PUT /products/_bulk
{ "create": { } }
{ "name": "bb","type":"1"}
{ "create": { } }
{ "name": "cc","type":"2"}
{ "create": { } }
{ "name": "dd","type":"1,2"}
{ "create": { } }
{ "name": "ee","type":"1,3"}
{ "create": { } }
{ "name": "ff","type":"3"}
```

#### 3.3、查看索引
```
GET /products/_search
{
  "query" : {
    "match" : { "type": "1" }
  }
}

PUT /products/_doc/_mapping
{
  "_doc": {
    "properties": {
      "name": {
        "type": "keyword"
      }
    }
  }
}

//精确匹配
GET /products/_search
{
  "query" : {
    "bool": {
      "must": [
        {
          "term.keyword" : { "name": "aaa" }
        }
      ]
    }
  }
}
```

#### 3.3、删除索引
```
DELETE /product/_doc/1
```
