[TOC]

## [Docker]

### 一、Docker知识

1. docker理念

   > **docker的理念：将应用和环境打包成镜像**

2. 镜像可以创建容器，容器还可以打开和关闭

3. 参数可以空格指定，也可以等号指定

   ```
   docker commit -a "gongwei" -m "拷贝文件" mytomcat tomcat:0.1
   
   docker commit -a="gongwei" -m="拷贝文件" mytomcat tomcat:0.1
   ```

4. docker pull redis

   其实不光安装redis，还会安装redis的运行环境，比如Linux系统，所以我们能通过docke exec -it myredis bash进入容器

   **docker的理念：将应用和环境打包成镜像。**

   ![image-20221231125748234](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231125748234.png)

5. docker run和docker start区别

   ```
   docker run 后面指定的是一个镜像
   而docker start指定的是一个容器 or 镜像名
   那么我使用docker run命令将他启动，使用docker ps 可以看到已经启动了一个springboot容器，然后我们停止它（docker stop）
   再使用docker start启动它，再使用docker ps可以看到之前停止的已经启动了
   ```

6. --help

   ![image-20221231034832972](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231034832972.png)
   
   上图表示-t和--timestamps作用一样

### 二、Docker命令

#### 1.帮助命令

1. docker --help

2. docker 命令 --help

3. docker文档

   ```
   https://docker.easydoc.net/doc/81170005/cCewZWoN/lTKfePfP
   ```

4. docker命令大全

   ```
   https://www.coonote.com/docker/docker-cmd-manual.html
   ```

5. docker文档

   ```
   https://www.coonote.com/docker/docker-cmd-manual.html
   ```

#### 2.镜像命令

1. 查看镜像：docker images

   ```sh
   docker images
   
   查看所有镜像id
   docker images -aq
   ```

2. docker search mysql

   ```
   搜索stars>3000的项目
   -filter=STARS=3000
   
   ```

3. docker pull mysql 默认拉取最新版本

   ```
   指定版本
   docker pull mysql:5.7 (版本一定要存在)
   ```

   ![image-20221231022436790](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231022436790.png)

   ```shell
   等价
   docker pull mysql
   docker pull docker.io/library/mysql:latest
   ```

4. 删除镜像：docker rmi 镜像名or镜像id

   ```shell
   删除一个镜像：
   docker rmi -f mysql
   
   删除多个镜像：
   docker rmi -f mysql redis
   
   删除全部镜像：
   docker rmi -f $(docker images -aq)
   
   -f, --force      Force removal of the image
   不加-f，默认不会删除正在运行的镜像，加上-f，就会删除正在运行的镜像
   ```

#### 3.容器命令

1. docker run：如何保证创建的容器不立刻退出

   ```
   以后台方式启动一个容器：
   docker run -d --name mycentos --rm centos
   会发现docker ps，容器自动退出了
   
   以交互方式启动一个容器：
   docker run -it --name mycentos --rm centos
   
   以交互、后台方式启动一个容器：
   docker run -it -d --name mycentos --rm centos
   ```

   ![image-20230101011226357](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101011226357.png)

   总结：

   ```SH
   如果不加-it（交互方式运行），那么容器会自动退出，不符合我们期望
   
   如果加了-it（交互方式运行），那么容器不会自动退出，同时不管我们添加bash没有，都会自动进入容器（不是我们期望的运行方式）
   
   如果加了-it -d（交互方式、后台运行），那么容器不会自动退出，也不会进入容器（符合我们期望）
   
   同时我们期望我们推出服务就关闭容器，可以添加--rm
   
   推荐运行方式
   docker run -it -d --name mycentos centos
   docker run -it -d --name mycentos --rm centos
   
   `docker run -d --name mycentos --rm centos
   `docker exec -it mycentos bash
   - 这种方式启动后，即使进入容器，再exit，也不会退出容器
   ```

2. docker run ：创建一个新的容器并运行一个命令（如果镜像没有，则会自动下载）

   ```
   docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
   ```

   ```
   --rm：Automatically remove the container when it exits，只会删除容器，不会删除镜像
   
   -a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
   
   -d: 后台运行容器，并返回容器ID；
   
   -i: 以交互模式运行容器，通常与 -t 同时使用；
   
   -P: 随机端口映射，容器内部端口随机映射到主机的端口
   
   -p: 指定端口映射，格式为：主机(宿主)端口:容器端口
   
   -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
   
   --name="nginx-lb": 为容器指定一个名称；
   
   --dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
   
   --dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
   
   -h "mars": 指定容器的hostname；
   
   -e username="ritchie": 设置环境变量；比如可以设置MySQL的登录密码：-e MYSQL_ROOT_PASSWORD=123456，-e可以重复，也就是可以配置多个参数，比如-e name=zhangsan -e age=10
   
   --env-file=[]: 从指定文件读入环境变量；
   
   --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
   
   -m :设置容器使用内存最大值；
   
   --net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
   
   --link=[]: 添加链接到另一个容器；
   
   --expose=[]: 开放一个端口或一组端口；
   
   --volume , -v: 绑定一个卷，可以绑定多个卷，docker run -v 主机路径:容器路径 -v 主机路径2:容器路径2 --name mynginx nginx，注意使用绝对路径
   ```

   使用docker镜像nginx:latest以后台模式启动一个容器,并将容器命名为mynginx。

   ```
   docker run --name mynginx -d nginx:latest
   ```

   使用镜像nginx:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。

   ```
   coonote@coonote:~$ docker run -it nginx:latest /bin/bash
   root@b8573233d675:/#
   
   
   从容器中退回主机
   exit
   ```

   运行centos，同时在/bin/sh中执行脚本

   ```
   docker run -d --name mycentos centos /bin/sh -c "while true;do echo nihao;sleep 1;done"  
   docker run -d --name mycentos centos /bin/bash -c "while true;do echo nihao;sleep 1;done" 
   ```

   运行nginx，并挂载端口

   ```
   docker run -d --name mynginx -p 8888:80 nginx
   ```

   ![image-20221231045135890](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231045135890.png)

   ![image-20221231050616146](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231050616146.png)

   

   停止容器后，自动删除容器：docker run -d --name myredis --rm redis

   ![image-20221231053550547](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231053550547.png)

   配置启动参数进行启动，比如配置MySQL密码-e

   ```
   docker run --name mymysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql
   
   -e可以重复，也就是可以配置多个参数,比如-e name=zhangsan -e age=10
   ```

   

3. docker ps：列出所有正在运行的容器信息。

   **NAMES:** 自动分配的容器名称。

   ```sh
   [~]$ docker ps                                                                                       
   CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
   ```

   列出最近创建的2个容器信息。

   ```SH
   [~]$ docker ps -n 2                                                                                                      
   CONTAINER ID   IMAGE     COMMAND                  CREATED       STATUS                   PORTS     NAMES
   fa7cfa0d74b3   redis     "docker-entrypoint.s…"   3 weeks ago   Exited (1) 3 weeks ago             keen_ardinghelli
   4212e914058a   redis     "docker-entrypoint.s…"   3 weeks ago   Exited (0) 3 weeks ago             peaceful_jennings
   ```

   列出最近创建的容器信息。

   ```SH
   [~]$ docker ps -a                                                                                                         
   CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS                   PORTS     NAMES
   fa7cfa0d74b3   redis          "docker-entrypoint.s…"   3 weeks ago   Exited (1) 3 weeks ago             keen_ardinghelli
   4212e914058a   redis          "docker-entrypoint.s…"   3 weeks ago   Exited (0) 3 weeks ago             peaceful_jennings
   7d0c2d1c75d3   redis:latest   "docker-entrypoint.s…"   3 weeks ago   Exited (0) 3 weeks ago             redis-9hrw
   ```

   ![image-20221231025053035](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231025053035.png)

4. **docker rm ：**删除一个或多个容器。

   ```
   docker rm -f 容器id/容器名称
   docker rm -f $(docker ps -aq) 删除所有容器
   docker rm -f $(docker ps -aq) 删除所有容器
   docker ps -aq | xargs docker rm 删除所有容器
   
   -f, --force     Force the removal of a running container (uses SIGKILL)
   不加-f，默认不会删除正在运行的容器，加上-f，就会删除正在运行的容器
   ```

   ![image-20221231032615306](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231032615306.png)

   

5. 启动容器，前面有docker run和docker start命令的区别

   ```
   docker start 容器id/容器名称
   ```

6. 重启容器（即使之前没启动也能重新启动）

   ```
   docker restart 容器id/容器名称
   ```

7. 停止容器

   ```
   docker stop 容器id/容器名称
   ```

8. 强制停止容器

   ```
   docker kill 容器id/容器名称
   ```

   ![image-20221231031659550](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231031659550.png)

9. 查看日志：docker logs -tf myredis

   ![image-20221231035820186](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231035820186.png)

10. 查看容器进程信息：docker top 容器id/容器名

    ```sh
    docker top myredis
    ```

11. 查看容器的元数据：docker inspect 容器id/容器名

    ```
    docker inspect myredis
    
    ```

12. 进入当前正在运行的容器：docker exec

    /bin/bash   bash   /bin/sh 都可以

    ![image-20221231041636696](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231041636696.png)

    docker attach 容器id/容器名 （也不需要/bin/sh）

    ![image-20221231041956820](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231041956820.png)

    ```
    docker exec -it mycentos /bin/sh：进入容器后开启一个新的终端，还可以在里面进行操作（常用）
    docker attach mycentos：进入容器正在执行的终端，不会启动新的进程
    ```

    ```
    docker exec -it mycentos 进入终端，如果没有添加bash，会报错
    
    docker exec -it mycentos bash（正确）
    ```

    ![image-20230101013350073](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101013350073.png)

13. 从容器内拷贝文件到主机：docker cp 容器id/容器名:容器内文件路径 主机文件路径

    ![image-20221231043640497](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231043640497.png)

14. 停止容器后，自动删除容器：docker run -d --name myredis --rm redis

    只会删除容器，不会删除镜像

    ![image-20221231053550547](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231053550547.png)

15. 查看内存占用：docker stats

    ![image-20221231122622817](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231122622817.png)

16. 查看镜像构建过程：docker history 镜像id/镜像名:TAG

    ```
    docker history centos
    ```

17. 给image指定tag（重新生成指定tag的image）：docker tag image_id new_image_name:TAG

    ```
    docker tag image_id new_image_name:TAG
    
    docker tag tomcat:0.1 tomcat:0.2     
    ```

    ![image-20230101000947743](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101000947743.png)

    

### 三、Docker部署几个小demo

#### 1.Docker部署Nginx（并手动修改index.html）

```
1.创建一个容器并运行
docker run -d --name mynginx -p 8888:80 --rm nginx

2.前端访问localhost:8888
curl localhost:8888
	 
```

![image-20221231114525243](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231114525243.png)



```
3.进入容器终端（exec方式，新开启一个终端）
bash   /bin/bash   /bin/sh   sh 都可以
docker exec -it mynginx bash

不要以这种方式进入nginx
docker attach nginx


4.修改index.html文件
cd /user/shared/html
or
cd /usr/share/nginx/html

cat index.html 或者是 vi index.html
如果是新的docker 容器 应该是没安装 Vim
	 需要执行命令
	 1  apt-get update
	 2  apt-get install vim
	 
vim index.html

:q! -不保存文件，强制退出 vim
:wq! -强制保存文件，退出 vim
:e! -放弃所有修改，从上次保存文件开始再编辑（vim进入文件后，i进入编辑模式，同时修改了部分代码，这时候想废弃改动，使用改命令，然后再使用i进入编辑修改即可）

5.前端强制刷新浏览器即可访问
command+shift+r

6.docker stop mynginx后，docker stat mynginx重启容器，修改的index.html仍然生效
```

![image-20221231120651395](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231120651395.png)

#### 2.Docker启动Es+Kibana

```
启动Es（启动会很耗内存）
docker run -d --name myelasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch

启动Es（限制内存）
docker run -d --name myelasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms=64m -Xmx=512m" elasticsearch

-Xmx    Java Heap最大值，默认值为物理内存的1/4，最佳设值应该视物理内存大小及计算机内其他内存开销而定；
-Xms    Java Heap初始值，Server端JVM最好将-Xms和-Xmx设为相同值，开发测试机JVM可以保留默认值；


访问
curl localhost:9200
```

#### 3.Docker启动Tomcat

1. 运行一个容器（自动下载镜像）

   ```
   docker run -d --name mytomcat -p 8080:8080 tomcat 
   ```

2. 访问8080端口

   ```
   curl localhost:8080
   
   访问失败，因为webapps下面为空（被阉割了）
   ```

   ![image-20221231131852388](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231131852388.png)

3. 我们需要进入容器，copy 文件夹

   ```
   docker exec -it mytomcat bash
   
   cp -r webapps.dist/* webapps/ （-r：递归拷贝）
   ```

4. 重新访问（可以正常访问）

   ```
   curl localhost:8080
   ```

   ![image-20221231132042065](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231132042065.png)

5. 把这次修改之后的容器打包成一个镜像：docker commit

   ```
   docker commit -a "作者" -m "注释" 容器id/容器名 镜像名:tag
   
   docker commit -a "gongwei" -m "拷贝文件" mytomcat tomcat:0.1
   ```

6. 使用新生成的镜像重新运行容器（可以正常访问，因为webapps里面有文件内容了）

   ![image-20221231132703568](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231132703568.png)

#### 4.使用数据卷进行MySQL数据同步

1. 创建容器

   ```
   不设置密码，运行失败
   docker run -d --name mymysql -p 3306:3306 mysql 
   
   设置密码，运行成功
   docker run -d --name mymysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
   ```

   ![image-20221231143555547](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231143555547.png)

2. 挂载目录，这样在容器里面创建的数据，即使删除容器，也不会丢失

   ```sh
   $ docker run --name mymysql -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
   ```

### 四、Docker容器数据卷

#### 1.使用数据卷进行文件挂载（双向同步）：docker run -v 

```
docker run -v /Users/gongwei/test:/test --name mynginx --rm -d nginx  

docker run -v 主机路径:容器路径 --name mynginx nginx

docker run -v 主机路径:容器路径 --name mynginx nginx

-v也可以重复，挂载多个目录
docker run -v 主机路径:容器路径 -v 主机路径2:容器路径2 --name mynginx nginx

如果路径不存在，会自动创建

路径都使用绝对路径
```

![image-20221231141416596](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231141416596.png)

#### 2.使用docker inspect 容器id/容器名查看挂载目录

```
docker inspect mynginx
```

![image-20221231144642216](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231144642216.png)

#### 3.具名挂载

1. 创建容器

   ```
   docker run -d --name mynginx -P -v /etc/nginx nginx
   
   -v 只填写容器路径，主机路径不需要填写
   ```

2. 使用docker inspect mynginx查看挂载路径

   ```
   docker inspect mynginx 
   ```

   ![image-20221231152045329](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231152045329.png)

3. 使用docker volume ls查看卷

   ![image-20221231152524915](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231152524915.png)

#### 2.具名挂载

1. 创建容器，并指定卷名

   ```
   docker run -d --name mynginx01 -P -v mynginx:/etc/nginx nginx
   
   -v 卷名:路径
   ```

2. 使用docker inspect mynginx查看挂载路径

   ```
   docker inspect mynginx01
   
   "Source": "/var/lib/docker/volumes/mynginx/_data"
   ```

   ![image-20221231152841087](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231152841087.png)

3. 使用docker volume ls查看卷

   ![image-20221231153105087](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231153105087.png)

#### 3.指定路径挂载

1. 创建容器，并指定卷名

   ```sh
   docker run -d --name mynginx01 -P -v /mynginx:/etc/nginx nginx
   
   -v 路径:路径
   ```

#### 4.权限（ro、rw）

1. 创建容器，并指定卷名，并指定权限

   ```sh
   docker run -d --name mynginx01 -P -v mynginx:/etc/nginx:ro nginx
   
   docker run -d --name mynginx01 -P -v mynginx:/etc/nginx:rw nginx
   
   
   ro：readonly（只读）
   rw：readwrite（可读可写）
   
   默认rw
   
   ro：只能从主机修改文件，不能从容器中修改文件
   rw：既能从主机修改文件，也能从容器中修改文件
   ```

#### 5.多个容器之间进行相互挂载：docker run --volume-from

> 双向共享数据

1. 创建数据共享卷

   ```
   docker run --name mymysql -d -p 3306:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql
   ```

2. 在创建一个容器，挂载到mymysql

   ```
   docker run --name mymysql02 -d -p 3306:3306 --volume-from mymysql01 -e MYSQL_ROOT_PASSWORD=123456 mysql
   ```

### 五、构建Dockerfile

> Dockerfile是一个脚本，通过该脚本可以生成一个镜像文件

#### 1.Dockerfile基础知识

1. 每个关键字必须大写
2. 从上到下依次执行
3. #表示注释
4. 每个指令都会创建一层

#### 2.Dockerfile命令详解

##### 1.Dockerfile命令详解

```sh
FROM						#基础镜像，当前新镜像是基于哪个镜像的
MAINTAINER			#镜像是谁写的 镜像维护者的姓名混合邮箱地址
RUN							#容器构建时需要运行的命令
EXPOSE					#当前容器对外保留出的端口
ADD							#将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包
WORKDIR					#指定在创建容器后，终端默认登录的进来工作目录，一个落脚点
ENV							#用来在构建镜像过程中设置环境变量
COPY						#类似ADD，拷贝文件和目录到镜像中！
VOLUME					#挂载的目录，容器数据卷，用于数据保存和持久化工作
CMD							#指定一个容器启动时要运行的命令，dockerFile中可以有多个CMD指令，但只有最后一个生效！		
ENTRYPOINT			#指定一个容器启动时要运行的命令！可以追加命令，和CMD一样
ONBUILD					#当构建一个被继承的DockerFile时运行命令，父镜像在被子镜像继承后，父镜像的ONBUILD被触发	
```

#### 3.构建一个简单的Dockerfile（制作镜像）

1. 构建一个简单的Dockerfile

   ```dockerfile
   FROM centos
   # 匿名挂载方式，可以挂载多个卷
   VOLUME ["volume1", "volume2"]
   CMD echo "build finsh"
   CMD /bin/bash
   # 或者
   # CMD ["/bin/bash"]
   ```

2. 构建镜像：docker build

   ```sh
   docker build -f Dockerfile -t centos:0.1 . 
   
   -f :指定要使用的Dockerfile路径，如果不指定-f，默认从当前文件下搜索Dockerfile进行build（名字必须是Dockerfile）
   -t :指定生成镜像名和tag
   
   注意后面需要带个点
   
   
   也可以重命名Dockerfile（dockerfile）
   docker build -f dockerfile -t centos:0.1 . 
   ```

3. 出现安全问题

   问题：

   - failed to solve with frontend dockerfile.v0: failed to read dockerfile: error from sender: open .Trash: operation not permitted

   解决办法：

   ```sh
   1.查看是否启动了rootless机制
   csrutil status
   >System Integrity Protection status: enabled.
   
   2.关闭此状态的方法: 重启电脑 -->  重启时，按住command + R键  -->  进入一个recovery 模式  -->  用户登录  -->  打开terminal, 输入disable的命令：
   csrutil disable
   
   3.重启电脑再查看状态
   csrutil status
   >System Integrity Protection status: disabled.
   ```

   ![img](https://www.52mac.com/data/attachment/forum/201810/15/175842k67xara8n8yahdh6.jpg)

   在终端中输入下面一行命令，然后按回车，如下图提示。
   csrutil disable

   ![img](https://www.52mac.com/data/attachment/forum/201810/15/175842et8xpn92o9lq9fzt.jpg)

4. docker run启动镜像，可以查看数据卷目录

   ![image-20221231200014051](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231200014051.png)

   匿名挂载

   ![image-20221231200106870](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231200106870.png)

##### 1.制作一个centos镜像

1. 我们可以先从远程拉取一个centos镜像，运行一下，发现很多命令都没有，比如vim

   ```
    docker run -it --name mycentos centos
   ```

   ![image-20221231205027719](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231205027719.png)

2. 自己制作Dockerfile

   ```dockerfile
   FROM centos
   MAINTAINER gongwei<1325075688@qq.com>
   ENV MYPATH /usr/local
   WORKDIR $MYPATH
   # centos系统里面自带vi命令
   RUN yum -y install vim
   RUN yum -y install net-tools
   EXPOSE 80 # docker run -p 80:80 --name mycentos centos，需要绑定端口进行访问
   # EXPOSE 80 80 这样我们run镜像后，就不需要指定端口映射了，但也仅能通过80访问，（还未测试，可不可以重新绑定端口）
   CMD echo $MYPATH
   CMD echo "build finsh"
   CMD /bin/bash
   ```

3. build 镜像

   ```
   docker build -f Dockerfile -t centos:0.2 .
   ```

4. 运行新容器

   ```
   docker run -it --name mycentos centos
   ```

5. 进入容器可以使用vim命令

   ```
   pwd：可以看见直接进入我们设定的工作目录 /usr/local
   
   
   vim test.java，改命令也可以正常使用
   ```


##### 2.制作一个Tomcat镜像

https://www.bilibili.com/video/BV1og4y1q7M4?p=30&spm_id_from=pageDriver&vd_source=58acbf449edd771737ee43a78ffdabf4

##### 3.CMD和ENTRYPOINT命令区别

1. 运行镜像时候，最后可以跟一个命令

![image-20221231211245347](/Users/gongwei/Library/Application Support/typora-user-images/image-20221231211245347.png)

2. 编写Dockerfile，centos镜像

   ```
   FROM centos
   CMD ["ls" "-a"]
   ```

3. 构建镜像

   ```
   docker build -f Dockerfile -t centos:0.1 .
   ```

4. 运行镜像

   ```
   docker run -it --name mycentos centos
   ```

   可以发现ls -a命令生效

5. 如果我们想追加一个命令

   ```
   docker run -it --name mycentos centos:0.1 -l
   
   组层命令：ls -la
   ```

   但是运行失败

   我们必须重新指定命令

   ```
   docker run -it --name mycentos centos ls -la
   ```

6. 如果我们使用ENTRYPOINT命令，就不需要重新指定命令了

   编写Dockerfile文件

   ```
   FROM centos
   ENTRYPOINT ["ls","-a"]
   ```

7. 使用新生成的Dockerfile进行构建镜像

   ```
   docker build -f Dockerfile -t centos:0.2 .
   ```

8. 运行镜像

   ```
   docker run -it --name mycentos centos:0.2
   ```

   发现ls -a命令生效

9. 如果我们想追加一个命令

   ```sh
   docker run -it --name mycentos centos:0.2 -l
   
   组成命令：ls -la
   ```

   发现运行成功

10. 总结：

    ```
    docker run时候追加的命令是直接拼接到ENTRYPOINT命令后面去的
    ```


#### 4.发布自己的镜像到docker hub

1. 先注册docker hub账户

2. 本地登录docker hub

   ```sh
   docker login -u zhangsan -p 123456
   
   docker logut
   ```

3. 查看本地镜像

   ```sh
   docker images
   ```

4. 推送到docker hub

   ```sh
   docker push REPOSITORY:TAG
   docker push IMAGE_NAME:TAG
   
   docker push mytomcat:0.1
   如果不指定tag，默认推送最新版本到docker hub
   ```

#### 5.发布自己的镜像到阿里云镜像

参考官方文档

#### 6.镜像和tar包的转换

##### 1.save

##### 2.load

### 六、Docker网络详解

#### 1.--link

1. docker容器和主机之间可以ping通

2. docker容器与容器之间可以ping通

   ![image-20230101021254432](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101021254432.png)

3. mac 查询ip地址

   ```
   ifconfig | grep inet
   ```

4. 容器内查询ip地址

   ```
   ip addr
   ```

5. 使用ip才能ping通，使用容器名ping不同

   ![image-20230101021500766](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101021500766.png)

6. docker run --link，则可以通过容器名进行ping通

   ```
   docker run -it -d --name mycentos01 --link mycentos centos
   
   docker exec -it mycentos01 ping mycentos
   ```

   ![image-20230101021905269](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101021905269.png)

   反向ping，ping不同，必须指定ip

   ![image-20230101022056459](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101022056459.png)

   --link探究

   ```
   docker run -it -d --name mycentos01 --link mycentos centos
   
   在mycentos01的/etc/hosts文件增加了一个映射关系，所以：
   docker exec -it mycentos01 ping mycentos （可以ping通）
   docker exec -it mycentos01 ping 容器id    （可以ping通）
   
   但是mycentos的/etc/hosts文件没有对应的映射关系，所以：
   docker exec -it mycentos ping mycentos01 （ping不通）
   需要修改mycentos的/etc/hosts文件才行
   ```

   ![image-20230101022749487](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101022749487.png)

#### 2.自定义网络互联

1. 自己创建网络：docker network create

   ```SH
   docker network create --driver bridge --subnet 172.20.0.0/16 --gateway 172.20.0.1 mynet
   
   查询创建的网络详情：
   docker network inspect mynet
   ```

   ![image-20230101112032068](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101112032068.png)

2. 创建容器，使用我们新创建的网络

   ```sh
   docker run -it -d --name mycentos --rm --net mynet centos 
   
   docker run -it -d --name mycentos01 --rm --net mynet centos 
   ```

3. 我们可以进入各自容器查看网络

   ```SH
   docker inspect mycentos
   
   docker inspect mycentos
   ```

4. 我们可以进入我们创建的网络进行查看配置

   ```sh
   docker network inspect mynet
   ```

   我们可以发现mynet多了两个网络配置

   ![image-20230101113352940](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101113352940.png)

5. 现在我们两个容器之间就可以通过容器名或者ip进行访问了

   ```
   docker exec -it mycentos ping mycentos01
   
   docker exec -it mycentos01 ping mycentos
   ```

6. 不过有个问题，主机不能ping通容器

7. docker network connect  网络 容器名

   ```SH
   新创建一个网络，现在有两个网络了mynet、mynet01
   docker network create --driver bridge --subnet 172.20.0.0/16 --gateway 172.20.0.1 mynet01
   
   创建容器mycentos02（使用mynet01）
   docker run -it -d --name mycentos02 --rm --net mynet01 centos
   
   现在mycentos02和mycentos和mycentos01是两个网络，不能相互访问，我们需要怎么做，他们之间才能相互访问呢？
   
   我们将mycentos02连接到网络mynet，这样mycentos02就可以访问mynet中的容器了，mycentos02就可以和mycentos01和mycentos进行相互访问了
   docker network connect  mynet mycentos02
   ```

### 七、使用docker部署redis集群（高可用）

- 视频链接：https://www.bilibili.com/video/BV1og4y1q7M4?p=38&vd_source=58acbf449edd771737ee43a78ffdabf4

### 八、将Django项目打包成docker镜像

- 视频链接：https://www.bilibili.com/video/BV1og4y1q7M4?p=39
- 一定要自己手动完成

