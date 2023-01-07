[toc]

### 一、DockerFile介绍

- dockerFile用来构建[docker镜像](https://so.csdn.net/so/search?q=docker镜像&spm=1001.2101.3001.7020)的文件！命令参数脚本

- 步骤：
  1.创建DockerFile文件
  2.docker build 构建成为一个镜像
  3.docker run 运行镜像
  4.docker push 发布镜像

### 二、DockerFile编写实操

- 编写一个自己的centos镜像
  基于centos，添加自己需要的环境，最后构建生成自己的镜像
  文件名:DockerFile

1. 编写DockerFile的文件

   ```dockerfile
   #基础镜像
   FROM centos
   #告诉镜像谁写的
   MAINTAINER chenmo<2514838942@qq.com>
   
   #设置环境变量
   ENV MYPATH /usr/local
   #设置镜像的工作目录
   WORKDIR $MYPATH
   
   #运行命令安装vim
   RUN yum -y install vim
   
   #暴露容器的端口
   EXPOSE 80
   
   CMD echo $MYPATH
   CMD echo "运行成功"
   CMD /bin/bash
   ```

   ```dockerfile
   # this is a dockerfile
   FROM centos:7
   MAINTAINER XD 123456@qq.com
   RUN echo "正在构建镜像！！！"
   WORKDIR /home/xdclass
   COPY 123.txt /home/xdclass
   RUN yum install -y net-tools
   ```

   ```dockerfile
   FROM  基于哪个镜像
   MAINTAINER  注明作者
   COPY  复制宿主机文件进入镜像不会解压（只能用相对路径，不能用绝对路径）
   ADD  复制宿主机文件进入镜像（假如文件是.tar.gz文件会解压）
   WORKDIR  指定工作目录，假如路径不存在会创建路径
   ENV  设置容器的环境变量（比较构造一个带jdk的centos镜像，那么就可以配置centos容器中的jdk环境变量）
   EXPOSE  暴露容器端口（比如配置一个带tomcat的centos镜像，那么就可以通过这个命令将tomcat的8080端口暴露出来）
   RUN  在构建镜像的时候执行，作用于镜像层面
   ENTRYPOINT  在容器启动的时候执行，作用于容器层，dockerfifile里有多条时只允许执行最后一条
   CMD  在容器启动的时候执行，作用于容器层，dockerfifile里有多条时只允许执行最后一条 容器启动后执行默认的命令或者参数，允许被修改
   
   命令格式（shell不允许携带cmd参数，exec允许携带cmd参数）：
   shell命令格式：RUN yum install -y net-tools
   exec命令格式：RUN [ "yum","install" ,"-y" ,"net-tools"]
   ```

   

2. 通过这个文件构建镜像

   ```dockerfile
   执行dockerfile文件并将生成的镜像命名为mycentos:v2，注意后面还有一个.表示当前路径下的dockerfile文件
   构建：docker build -t mycentos:v2 .
   
   
   docker build -f DockerFile -t 镜像名:[tag] .
   -f 表示要运行的文件名字，这儿指定 DockerFile
   ```

3. 运行进入容器

   ```dockerfile
   #运行镜像文件
   docker run -it REPOSITORY:TAG
   
   ```

4. 查看所有镜像

   ```dockerfile
   docker images
   ```

5. 查看指定镜像的创建历史

   ```dockerfile
   docker history 镜像id
   ```

### 三、Docker问题

1. 运行docker镜像时候，出现找不到目录

   - docker 2022-11-16 14:54:26 python: can't open file '/src/main.py': [Errno 2]

   - 解决方案

     ```dockerfile
     在DockerFile中，cmd给出的命令没有给出完整的main函数路径，导致找不到主函数
     
     #基于的基础镜像
     FROM python:3.6.8
     #代码添加到code文件夹
     ADD . /code
     # 设置code文件夹是工作目录
     WORKDIR /code
     # 安装支持
     RUN pip install -r requirements.txt -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
     CMD ["python", "/code/src/main.py"]
     
     
     改动：
     CMD ["python", "/src/main.py"]
     
     CMD ["python", "/code/src/main.py"]
     ```

2. 如何运行多个命令

   ```dockerfile
   #启动好几个服务时，可以用CMD 执行一个脚本，在脚本中启动好几个服务
   
   CMD source /start_server.sh
   或者
   CMD ["sh","run.sh"]
   ```

   ```shell
   # start mala-core rpc server
   python manage.py start_open_server &
   
   # start mala-core cron_task server
   python manage.py start_cron_task &
   ```

   