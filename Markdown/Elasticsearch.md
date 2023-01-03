## 文章目录

[TOC]

### 一、Elasticsearch

#### 1.作者信息

| 作者                                               | 视频链接                                                     | 学习时间          | 备注         |
| -------------------------------------------------- | ------------------------------------------------------------ | ----------------- | ------------ |
| [遇见狂神说](https://space.bilibili.com/95256449)  | [Elasticsearch](https://www.bilibili.com/video/BV17a4y1x7zq?p=1&vd_source=58acbf449edd771737ee43a78ffdabf4) | 2023.1.2~2023.1.4 | 资源不是很好 |
| [编程不良人](https://space.bilibili.com/352224540) | [Elasticsearch](https://www.bilibili.com/video/BV1SQ4y1m7Ds?p=10&spm_id_from=pageDriver&vd_source=58acbf449edd771737ee43a78ffdabf4) | 2023.1.4~2023.1.  | 资源不错     |
|                                                    |                                                              |                   |              |

#### 2.本文主要内容

> 本文主要介绍Elasticsearch基础语法知识，包括可视化工具Kibana

### 二、启动Elasticseach

#### 1.本地启动

##### 1.启动

1. 前台启动

   ```sh
   1. cd /Elasticsearch/bin
   2. ./elasticseach (一定需要./，不然运行不了)
   ```

2. 本地启动报错

   - 问题

     ```
     [root@lysw vm]# curl 127.0.0.1:9200
     curl: (52) Empty reply from server
     ```

   - 解决

     ```
     ##进入容器，修改/elasticsearch/config/elasticsearch.yml
     [root@lysw config]# docker exec -it d31948cd75ef /bin/bash
     elasticsearch@d31948cd75ef:~$ vi config/elasticsearch.yml
     将其中的
     xpack.security.enabled: true
     #修改为，其他不要动，否则ES容器直接启动不了了
     xpack.security.enabled: false
     ```

3. 后台启动

   ```sh
   ./elasticsearch -d
   ```

4. 启动之后，日志里面出现start

##### 2.关闭

1. 前台启动关闭方式

   ```sh
   直接关闭窗口
   ```

2. 后台启动关闭方式

   ```sh
   ps aux | grep elasticsearch
   用户名之后的就是进程号
   
   kill -9 pid
   ```

#### 2.docker启动

```markdown
# 1.获取镜像
- docker pull elasticsearch:7.14.0

# 2.运行es
- docker run --name myelasticsearh -d -p 9200:9200 -p 9300:9300  -e "discovery.type=single-node"  elasticsearch:7.14.0

# 3.访问ES
- http://10.15.0.5:9200/
```



### 三、Kibana

#### 1.docker方式安装

```markdown
# 1.获取镜像
- docker pull kibana:7.14.0

# 2.运行kibana
- docker run -d  --name mykibana -p 5601:5601 kibana:7.14.0

# 3.进入容器修改kibana.yml
- docker exec -it mykibana bash  # 进入容器
- 修改config/kibana.yml配置
- server.host: "0.0.0.0"                		# 开启kibana远程访问
- elasticsearch.hosts: ["http://主机的ip:9200"]   #ES服务器地址

# 4.重启mykibana
- docker rm -f mykibana
- docker run -d  --name mykibana -p 5601:5601 kibana:7.14.0

# 5.外部访问
- 发现无法访问，打印日志，发现kibana没有启动
- docker logs -tf mykibana

# 6.查明原因
- 经查，发现kibana.yml配置自动改回去了，所以打算使用外卷的方式解决

# 7.拷贝kibana.yaml到到主机
- cd /Users/gongwei/
- docker cp mykibana:/usr/share/kibana/config/kibana.yml ./

# 8.在主机修改kibana.yml配置
- server.host: "0.0.0.0"                		# 开启kibana远程访问
- elasticsearch.hosts: ["http://主机的ip:9200"]   #ES服务器地址

# 9.基于数据卷加载配置文件方式运行
`docker run -d -v /Users/gongwei/kibana.yml:/usr/share/kibana/config/kibana.yml  --name mykibana -p 5601:5601 kibana:7.14.0

# 10.外部访问（成功）
- curl http://localhost:5601
```

### 四、核心概念

### 索引<Index> 

**`一个索引就是一个拥有几分相似特征的文档的集合`**。比如说，你可以有一个商品数据的索引，一个订单数据的索引，还有一个用户数据的索引。**`一个索引由一个名字来标识`**`(必须全部是小写字母的)`**，**并且当我们要对这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。

不能修改

### 映射<Mapping>

**`映射是定义一个文档和它所包含的字段如何被存储和索引的过程`**。在默认配置下，ES可以根据插入的数据`自动地创建mapping，也可以手动创建mapping`。 mapping中主要包括字段名、字段类型等 

不能删除和修改

### 文档<Document>

**`文档是索引中存储的一条条数据。一条文档是一个可被索引的最小单元`**。ES中的文档采用了轻量级的JSON格式数据来表示。 

