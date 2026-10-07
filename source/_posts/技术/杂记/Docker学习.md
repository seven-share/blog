---
title: Docker学习
date: 2019-06-02 17:00
top: 0
categories: 
  - 技术
  - 杂记
---
### Docker学习
命令学习
docker run ubuntu echo 'hello world'
ubuntu是镜像名字，后为执行语句
一次执行，容器就会消失

docker run -i -t ubuntu /bin/bash
-i 交互
-t ttl进程
启动交互式，镜像为ubuntu的容器的终端
exit退出，-it也可

自定义容器名字
docker run --name 自定义的名字 ubuntu echo 'hello world'

docker ps
显示正在运行中的容器
-a显示全部，包括之前停止的容器
-l，显示之前最新的容器

docker inspect 容器名字
获取镜像或容器的元数据

重新启动之前的停止的容器
docker start -i 容器名
-i以交互的形式重启

删除容器(必须是已经停止的)
docker rm 容器名


守护式容器
docker run -i -t ubuntu /bin/bash
ctrl+q或p退出，则会在后台运行

加入-d直接进入守护式容器
docker run -d ubuntu /bin/sh -c '执行语句'

重新进入该容器
docker attach 容器名

查看该容器的运行情况
docker logs [-f][-i][--tail 数字] 容器名
-f follow 实时显示
-t timestamps 显示时间戳
--tail 显示最新的数量，默认是所有

查看运行中容器的进程
docker top 容器名

启动容器中的新进程
docker exec -i -t 容器名 命令

停止运行守护容器
docker kill 容器名
直接停止，较为强势
docker stop 容器名
静待容器自己停止

镜像
查看镜像
docker images [options] [respository]
-a all
-f filter 筛选
--no-trunc 不截断显示id
-q quiet 只显示id
respository：例如ubuntu，会只显示该仓库的镜像

查看镜像的详细信息
docker inspect 仓库名：标签名
docker inspect id

删除镜像
docker rmi 镜像：标签
           或id
删除所有镜像
docker rmi $(docker images -q ubuntu)

查找镜像
docker search 镜像名
--automate 只展示自动构建
--no-trunc 不截断显示
-s 数字 最低星级

下载镜像
docker pull 仓库名：标签名字
上传镜像
docker push 仓库名：标签

使用commit构建镜像
docker commit 提交的容器名 镜像名字（将该容器打包为镜像）
    -a 作者
    -m 保存的信息
    -p 是否暂停容器

使用dockerfile构建镜像
首先创建dockerfile文件
docker build path
path是dockerfile的所在地址
-t 标签名
默认是使用吧缓存进行构建
有些数据会不是最新的
--no-cache 不使用缓存
docker history images

dockerfile内容
#开头是注释
指令
FROM
FROM 镜像：标签

MAINTAINER 作者信息 邮箱等

RUN
镜像构建时执行的命令
shell模式
exec模式

EXPOSE
指定镜像使用的端口号

CMD
在容器运行时的默认命令，如果启动容器docker run时有命令，则会被覆盖

ENTERYPOINT
启动容器docker run时有命令不会执行，会执行ENTERYPOINT

ADD 复制文件，包含类似tar的解压功能
ADD 要复制的文件地址（相对dockerfile的文件路径） 要复制到位置（绝对路径）
COPY 单纯的复制文件

VOLUME
指定卷，作为数据共享

WORKDIR
指定工作路径，一般为绝对路径

ENV
环境变量

USER
指定运行用户

ONBUILD
镜像触发器
当一个镜像被其他镜像作为基础镜像的时候被执行
会在构建过程中插入命令


查看docker的情况
ps -ef|grep docker
sudo status docker

sudo service docker stop
sudo service docker start
sudo service docker restart
docker -d [options]
守护进程启动配置文件 /etc/default/docker
配置文件添加
DOCKER_OPTS=" --label name=abc"
给服务器docker添加名字，以示区分，本地和远程都起好名字，方便区分

默认的docker守护进程 -H tcp://host:port
                      unix:///path/to/socket
                      fd://*or fd://socketfd
                    默认的是unix:///var/run/docker.sock
给远程服务器添加配置
DOCKER_OPTS=" --label name=abc -H tcp:///0.0.0.0:2375"
保存，退出，重启docker使命令生效

远程客户端访问
docker -H tcp://远程机的ip:上面设置的端口 [options]
设置环境变量简化访问
export DOCKER_HOST='tcp://远程机的ip:上面设置的端口'
docker info会默认访问远程
将export DOCKER_HOST=''置空则会默认访问本地

使用远程机的时候发现不支持本地访问docker
需要在配置文件添加DOCKER_OPTS=" --label name=abc -H tcp:///0.0.0.0:2375 -H unix:///var/run/docker.sock"


容器的互联
默认是互联，但是重启后容器ip会发生变化，使用link指定连接的容器你
docker run --link=容器名：别名 images command

不允许互联
修改默认配置文件 /etc/default/docker
加上DOCKER_OPTS='icc=false'

允许特定的连接
icc=false iptables=true
使用link指定

数据卷
docker run -v ~/datavolume:/data -it unbuntu /bin/bash
-v 宿主机放数据卷的位置，容器放数据卷的位置
docker run -v ~/datavolume:/data:权限 -it unbuntu /bin/bash

数据卷容器
首先一个容器挂在了数据卷
然后再新建容器是--volumes-from 该容器名
例：docker run --volumes-from 该容器名 -it unbuntu /bin/bash
可以共享该数据卷
使用数据卷容器新建的容器，是使用该数据卷容器的配置
数据卷容器被删除后，仍可以使用该数据卷

数据备份和还原
docker run --volumes-from 容器名 -v 宿主机存放的数据卷位置：容器数据卷存放位置 unbuntu tar cvf 宿主机存放数据卷位置 需要备份的目录
数据还原 将cvf改为xvf即可

