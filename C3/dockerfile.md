# Dockerfile 语法和最佳实践

## FROM
```Dockerfile
FROM scratch    # 制作base image
FROM centos
FROM ubuntu:14.04
```
> 尽量使用官方的image作为base image！

## LABEL
```Dockerfile
LABEL maintainer="haishenmingx@gmail.com"
LABEL version="1.0"
LABEL description="This is description"
```
> Metadata不能少！

## RUN
```Dockerfile
RUN yum update && yum install -y vim \
    python-dev     # 反斜线换行
```
> 为了美观，复杂的RUN请使用反斜线换行！
> 避免无用分层，合并多行命令为一行！

## WORKDIR
``` Dockerfile
WORKDIR /root
WORKDIR /test  # 如果没有会自动创建test目录
```
> 使用WORKDIR，不要使用RUN cd!
> 尽量使用绝对路径！

## ADD and COPY
```Dockerfile
ADD hello /      # 添加hello到根目录
ADD test.tar.gz / # 添加到根目录并解压

WORKDIR /root
ADD hello test/  # /root/test/hello

WORKDIR /root
COPY hello test
```
> ADD 可以解压缩，COPY不可以
> 大部分情况，COPY优于ADD！因为ADD还有一些额外的功能！
> 添加远程文件/目录请使用curl或者wget！


## ENV
```Dockerfile
ENV MYSQL_VERSION 5.6    # 设置常量
RUN apt install -y mysql-server="${MYSQL_VERSION}" \
    && rm -rf /var/lib/apt/lists/*    # 引用常量
```
> 尽量使用ENV增加可维护性！

## VALUME and EXPOSE
存储和网络

## CMD and ENTRYPOINT
**RUN**: 执行命令并创建新的Image Layer <br>
**CMD**: 设置容器启动后默认执行的命令和参数, 如果docker run指定了其他命令，CMD命令将会被忽略，如果定义了多个CMD，只有最后一个会执行 <br>
**ENTRYPOINT**: 设置容器启动时运行的命令， 让容器以应用程序或者服务的形式运行，不会被忽略，一定会执行，**最佳实践**: 写一个shell脚本作为entrypoint <br>
```Dockerfile
# shell 格式
RUN apt install -y vim
CMD echo "hello docker"
ENTRYPOINT echo "hello docker"

# Exec 格式
RUN ["apt", "install", "-y", "vim"]
CMD ["/bin/echo"， "hello docker"]
ENTRYPOINT ["/bin/echo"， "hello docker"]
```