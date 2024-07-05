# 一、ElasticSearch知识体系

## 一、官方文档

- [低版本中文文档](https://www.elastic.co/guide/cn/elasticsearch/guide/current/foreword_id.html)
- [最新版官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/index.html)
- [Java操作ES的客户端文档](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high.html)
- [ Kibana数据可视化界面使用官方文档 ](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high.html)

## 二、参考文档

- [ElasticSearch知识体系详解](https://www.pdai.tech/md/db/nosql-es/elasticsearch.html)
- [SpringBoot检索篇-整合ElasticSearch7.6.2](https://blog.csdn.net/weixin_41105242/article/details/107711634)

## 三、ElasticSearch前景

1. 在当前软件行业中，搜索是一个软件系统或平台的基本功能， 学习ElasticSearch就可以为相应的软件打造出良好的搜索体验
2. 其次，ElasticSearch具备非常强的大数据分析能力。虽然Hadoop也可以做大数据分析，但是ElasticSearch的分析能力非常高，具备Hadoop不具备的能力。比如有时候用Hadoop分析一个结果，可能等待的时间比较长
3. ElasticSearch可以很方便的进行使用，可以将其安装在个人的笔记本电脑，也可以在生产环境中，将其进行水平扩展
4. 国内比较大的互联网公司都在使用，比如小米、滴滴、携程等公司。另外，在腾讯云、阿里云的云平台上，也都有相应的ElasticSearch云产品可以使用
5. 在当今大数据时代，掌握近实时的搜索和分析能力，才能掌握核心竞争力，洞见未来

## 四、ElasticSearch概述

1. ELK 是 Elasticsearch、Logstash、Kibana 三大开源框架的首字母大写简称。市面上也被称为Elastic Stack 
2. ElasticSearch 是一款非常强大的、基于Lucene的开源搜索及分析引擎；它是一个实时的分布式搜索分析引擎，它能让你以前所未有的速度和规模，去探索你的数据 
3. Elasticsearch 被用作全文检索、结构化搜索、分析以及这三个功能的组合 
4. Elasticsearch 是使用 Java 编写的，它的内部使用 Lucene 做索引与搜索，但是它的目的是使全文检索变得简单，通过隐藏 Lucene 的复杂性，取而代之的提供一套简单一致的 RESTful API 
5. Elasticsearch 不仅仅是 Lucene，并且也不仅仅只是一个全文搜索引擎。 它可以被下面这样准确的形容：
   - 一个分布式的实时文档存储，每个字段可以被索引与搜索
   - 一个分布式实时分析搜索引擎
   - 能胜任上百个服务节点的扩展，并支持 PB 级别的结构化或者非结构化数据 

## 五、Elasticsearch和Solr比较

1. Elasticsearch 基本是开箱即用，非常简单。Solr 安装略微复杂一丢丢
2. Solr 利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能
3. Solr 支持更多格式的数据，比如JSON、XML、CSV，而 Elasticsearch 仅支持 JSON 文件格式
4. Solr 官方提供的功能更多，而 Elasticsearch 本身更注重于核心功能，高级功能多有第三方插件提供，例如图形化界面需要 kibana 友好支撑
5. Solr 查询快，但更新索引时慢（即插入删除慢），用于电商等查询多的应用；
   - Elasticsearch 建立索引快，即查询慢，即实时性查询快，用于facebook、新浪等搜索
   - Solr 是传统搜索应用的有力解决方案，但 Elasticsearch 更适用于新兴的实时搜索应用
6. Solr比较成熟，有一个更大，更成熟的用户、开发和贡献者社区，而 Elasticsearch 相对开发维护者较少，更新太快，学习使用成本较高

# 二、ES基础概念

## 一、ES核心名词

1. **Near Realtime（NRT）**： 近实时。数据提交索引后，立马就可以搜索到
2. **Cluster 集群**：一个集群有一个唯一的名字标识，默认为“elasticsearch”。集群名称非常重要，具有相同集群名的节点才会组成一个集群。集群名称可以在配置文件中指定
3. **Node 节点**：存储集群的数据，参与集群的索引和搜索功能。像集群有名字，节点也有自己的名称，默认在启动时会以一个随机的UUID的前七个字符作为节点的名字，也可以为其指定任意的名字。通过集群名在网络中发现同伴组成集群。一个节点也可是集群
4. **Index 索引**：一个索引是一个文档的集合。每个索引有唯一的名字，通过这个名字来操作它。一个集群中可以有任意多个索引
5. **Type 类型**：指在一个索引中，可以索引不同类型的文档，如用户数据、博客数据。从6.0.0版本起已废弃，一个索引中只存放一类数据
6. **Document 文档**：被索引的一条数据，索引的基本信息单元，以JSON格式来表示。
7. **Shard 分片**：在创建一个索引时可以指定分成多少个分片来存储。每个分片本身也是一个功能完善且独立的“索引”，可以被放置在集群的任意节点上
8. **Replication 备份**：一个分片可以有多个备份（副本）

## 二、ES名词和数据库名词对比

![](../../../TyporaImage/es-introduce-1-3.png)

## 三、RESTful

1. RESTful 指的是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是 RESTful。Web 应用程序最重要的 REST 原则是，客户端和服务器之间的交互在请求之间是无状态的。从客户端到服务器的每个请求都必须包含理解请求所必需的信息。如果服务器在请求之间的任何时间点重启，客户端不会得到通知。此外，无状态请求可以由任何可用服务器回答，这十分适合云计算之类的环境。客户端可以缓存数据以改进性能
2. 在服务器端，应用程序状态和功能可以分为各种资源。资源是一个有趣的概念实体，它向客户端公开。资源的例子有：应用程序对象、数据库记录、算法等等。每个资源都使用 URI (Universal Resource Identifier) 得到一个唯一的地址。所有资源都共享统一的接口，以便在客户端和服务器之间传输状态。使用的是标准的 HTTP 方法，比如 GET、PUT、POST 和 DELETE
3. 在 REST 样式的 Web 服务中，每个资源都有一个地址。资源本身都是方法调用的目标，方法列表对所有资源都是一样的。这些方法都是标准方法，包括 HTTP GET、POST、 PUT、DELETE，还可能包括 HEAD 和 OPTIONS。简单的理解就是，如果想要访问互联网上的资源，就必须向资源所在的服务器发出请求，请求体中必须包含资源的网络路径，以及对资源进行的操作（增删改查）

# 三、ES安装

## 一、官网相关教程

1. [官方网站](https://www.elastic.co/cn/)
2. [官方2.x中文教程中安装教程](https://www.elastic.co/guide/cn/elasticsearch/guide/current/running-elasticsearch.html)
3. [官方ElasticSearch下载地址](https://www.elastic.co/cn/downloads/elasticsearch)
4. [官方Kibana下载地址](https://www.elastic.co/cn/downloads/kibana)

## 二、安装ElasticSearch

### 一、安装注意事项

- ElasticSearch 是使用 java 开发的，且本版本的 es 需要的 jdk 版本要是 1.8 以上，所以安装
- ElasticSearch 之前保证 JDK1.8+ ，并正确的配置好JDK环境变量，否则会启动ElasticSearch失败 

### 二、Windows版本安装

#### 一、ElasticSearch安装

1. [Win版本下载ElasticSearch](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.1.zip)

2. 文件目录

   ```tex
   bin：启动文件
   config：配置文件
       log4j2.properties：日志配置文件
       jvm.options：java虚拟机的配置
       elasticsearch.yml：es的配置文件
   data：索引数据目录
   lib：相关类库Jar包
   logs：日志目录
   modules：功能模块
   plugins：插件
   ```

3. 启动方式：双击 ElasticSearch 下的 bin 目录中的 elasticsearch.bat 启动 

4. 访问地址：http://localhost:9200 

#### 二、ES图形化插件Head安装

1. 注意点：需要NodeJS环境

2. Head 是 elasticsearch 的集群管理工具，可以用于数据的浏览查询！被托管在 github 上面 

3. [下载地址](https://github.com/mobz/elasticsearch-head/)

4. 解压并安装

   ```shell
   #安装依赖！
   cnpm install
   #运行
   npm run start
   ```

5. 修改配置文件 `elasticsearch.yml`。由于 ES 进程和客户端进程端口号不同，存在跨域问题，所以我们要在 ES 的配置文件中配置下跨域问题

   ```yaml
   # 跨域配置：
   http.cors.enabled: true
   http.cors.allow-origin: "*"
   ```

6. 启动测试

   - 启动 ElasticSearch
   - 并使用head工具进行连接测试！
   - 访问url：http://localhost:9100/

#### 三、Kibana安装

1. 注意版本对应关系：https://www.elastic.co/cn/downloads/kibana 

2. 解压后进入，双击`kibana.bat`启动服务就可以了，ELK基本上都是拆箱即用的

3. 访问地址：http://localhost:5601/。Kibana 会自动去访问9200，也就是 elasticsearch 的端口号（当然 elasticsearch 这个时候必须启动着），然后就可以使用 kibana 了！ 

4. 访问界面是英文，可修改成中文。只需要在配置文件 kibana.yml 中加入 

   ```shell
   i18n.locale: "zh-CN"
   ```

5. 重启即可

### 三、Linux版本安装

#### 一、ElasticSearch安装

1. 进入一个目录，进行下载

   ```shell
   curl -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.1-linux-x86_64.tar.gz
   
   ```

2. 解压

   ```shell
   tar zxvf /opt/elasticsearch-7.6.1-linux-x86_64.tar.gz
   ```

3. 增加 elasticSearch 用户。必须创建一个非 root 用户来运行 ElasticSearch 。 如果使用 root 用户来启动 ElasticSearch ，启动会报错 。ElasticSearch5 及以上版本，基于安全考虑，强制规定不能以 root 身份运行  

   ```shell
   [root@VM-0-6-centos elasticsearch-7.6.1]# useradd elasticsearch
   [root@VM-0-6-centos elasticsearch-7.6.1]# passwd elasticsearch
   Changing password for user elasticsearch.
   New password: 
   BAD PASSWORD: The password contains the user name in some form
   Retype new password: 
   passwd: all authentication tokens updated successfully.
   
   ```

4. 修改目录权限至新增的 elasticsearch 用户。没权限启动会报错，需要在 root 用户下执行命令

   ```shell
   [root@VM-0-6-centos ~]# chown -R elasticsearch /opt/elasticsearch-7.6.1
   ```

5. 启动

   ```shell
   #切换用户
   su elasticsearch
   #启动
   ./bin/elasticsearch -d
   ```

6. 查看安装是否安装

   ```shell
   [root@VM-0-6-centos ~]# netstat -ntlp | grep 9200
   tcp6       0      0 127.0.0.1:9200          :::*                    LISTEN      11601/java          
   tcp6       0      0 ::1:9200                :::*                    LISTEN      11601/java          
   #	访问成功
   [root@VM-0-6-centos ~]# curl 127.0.0.1:9200
   {
     "name" : "VM-0-6-centos",
     "cluster_name" : "elasticsearch",
     "cluster_uuid" : "sLGiUbfQQ5-btWB9QiOnxg",
     "version" : {
       "number" : "7.6.1",
       "build_flavor" : "default",
       "build_type" : "tar",
       "build_hash" : "aa751e09be0a5072e8570670309b1f12348f023b",
       "build_date" : "2020-02-29T00:15:25.529771Z",
       "build_snapshot" : false,
       "lucene_version" : "8.4.0",
       "minimum_wire_compatibility_version" : "6.8.0",
       "minimum_index_compatibility_version" : "6.0.0-beta1"
     },
     "tagline" : "You Know, for Search"
   }
   
   
   ```

#### 二、Kibana安装

- 安装 Kibana 及 ElasticSearch 相关配置请访问：https://www.pdai.tech/md/db/nosql-es/elasticsearch-x-install.html 

### 四、Docker版本安装

#### 一、基础安装

1. [官方文档](https://www.elastic.co/guide/en/kibana/current/docker.html)

2. 创建网络

   ```shell
   docker network create elastic
   ```

3. 加上内存限制启动ES

   ```shell
   docker run -d --name es01-test --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.1
   ```

4. 启动Kibana

5. 可视化工具 Elasticsearch-head（可选） 

   ```shell
   docker run -d --name head-test --net elastic -p 9100:9100  mobz/elasticsearch-head:5
   ```

#### 二、配置密码访问

1. 修改ES配置：使用基本许可证时，默认情况下禁用 ElasticSearch 安全功能。[相关文档](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/security-minimal-setup.html) 

2. 进入正在运行的容器

   ```shell
   docker exec -it 容器id /bin/bash
   #安装 vim
   yum -y install vim
   ```

3. 更改`config/elasticsearch.yml`文件，设置`xpack.security.enabled`为 true

   ```shell
   [root@06c5914709a5 config]# vim elasticsearch.yml
   [root@06c5914709a5 config]# cat elasticsearch.yml
   cluster.name: "docker-cluster"
   network.host: 0.0.0.0
   xpack.security.enabled: true
   
   ```

4. 重启ES容器

   ```shell
   [root@VM-0-6-centos z-test]# docker restart es01-test
   es01-test
   ```

5. 进入容器设置各个组件的密码

   ```java
   [root@06c5914709a5 elasticsearch]# ./bin/elasticsearch-setup-passwords interactive
   Initiating the setup of passwords for reserved users elastic,apm_system,kibana,logstash_system,beats_system,remote_monitoring_user.
   You will be prompted to enter passwords as the process progresses.
   Please confirm that you would like to continue [y/N]y
   
   
   Enter password for [elastic]: 
   Reenter password for [elastic]: 
   Enter password for [apm_system]: 
   Reenter password for [apm_system]: 
   Enter password for [kibana]: 
   Reenter password for [kibana]: 
   Enter password for [logstash_system]: 
   Reenter password for [logstash_system]: 
   Enter password for [beats_system]: 
   Reenter password for [beats_system]: 
   Enter password for [remote_monitoring_user]: 
   Reenter password for [remote_monitoring_user]: 
   Changed password for user [apm_system]
   Changed password for user [kibana]
   Changed password for user [logstash_system]
   Changed password for user [beats_system]
   Changed password for user [remote_monitoring_user]
   Changed password for user [elastic]
   
   ```

6. 修改`kibana.yml`

7. 进入容器

8. 添加配置：`elasticsearch.username: "elastic"`

   ```shell
   bash-4.2$ vi kibana.yml
   bash-4.2$ cat kibana.yml
   #
   # ** THIS IS AN AUTO-GENERATED FILE **
   #
   
   # Default Kibana configuration for docker target
   server.name: kibana
   server.host: "0"
   elasticsearch.hosts: [ "http://elasticsearch:9200" ]
   xpack.monitoring.ui.container.elasticsearch.enabled: true
   elasticsearch.username: "elastic"
   
   ```

9. 创建 kibana keystore 

   ```shell
   ./bin/kibana-keystore create
   ```

10. 在kibana keystore 中添加密码

    ```shell
    ./bin/kibana-keystore add elasticsearch.password
    ```

11. 再次访问：`ip:9200`，则需要输入账号和密码。账号为：elastic

# 四、ES索引和文档的基本操作

  

