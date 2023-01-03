## 文章目录

[TOC]

### 一、Elasticsearch

#### 1.作者信息

| 作者                                              | 视频链接                                                     | 学习时间  | 备注 |
| ------------------------------------------------- | ------------------------------------------------------------ | --------- | ---- |
| [遇见狂神说](https://space.bilibili.com/95256449) | [Elasticsearch](https://www.bilibili.com/video/BV17a4y1x7zq?p=1&vd_source=58acbf449edd771737ee43a78ffdabf4) | 2023-1-2~ |      |
|                                                   |                                                              |           |      |
|                                                   |                                                              |           |      |

#### 2.本文主要内容

> 本文主要介绍Elasticsearch基础语法知识

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

   ```
   直接关闭窗口
   ```

2. 后台启动关闭方式

   ```sh
   ps aux | grep elasticsearch
   用户名之后的就是进程号
   
   kill -9 pid
   ```

#### 2.docker启动

