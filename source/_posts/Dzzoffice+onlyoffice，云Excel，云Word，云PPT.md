---
title: Dzzoffice+onlyoffice，云Excel，云Word，云PPT
---

##### 1：配置网络

```
cd /etc/netplan
ls -l
sudo nano 50-cloud-init.yaml
```



```
network:
	version: 2
    renderer: networkd
    ethernets:
        ens33:
            dhcp4: false
            dhcp6: false
            addresses: [192.168.159.100/24]
            routes:
              - to: default
                via: 192.168.159.2
            nameservers:
                addresses: [8.8.8.8,114.114.114.114]
                search: []
```

##### 重启应用

```
sudo netplan apply
```

##### 2：修改/etc/systemd/resolved.conf

```
DNS=8.8.8.8 114.114.114.114
```

##### 重启应用

```
sudo systemctl restart systemd-resolved
sudo systemctl enable systemd-resolved
```

##### 3：更换阿里源，打开配置文件

```
sudo vim /etc/apt/sources.list.d/ubuntu.sources
```

##### 粘贴以下配置，并保存退出

```
Types: deb
URIs: http://mirrors.aliyun.com/ubuntu/
Suites: noble noble-updates noble-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

##### 4：更新源列表

```
sudo apt-get update
```

##### 更新系统软件包

```
sudo apt-get upgrade
```

##### 5：安装必要的依赖包，这些包允许APT通过HTTPS使用仓库：

```
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
```

##### 导入Docker的官方GPG密钥以确保下载的包的真实性：

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

##### 添加Docker的官方APT仓库到系统中：

```
sudo add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

##### 更新APT包索引以确保APT使用新添加的仓库：

```
sudo apt-get update
```

##### 安装Docker CE：

```
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

##### 验证Docker是否安装成功并运行：

```
sudo systemctl status docker
```

##### 更换docker镜像源

```
sudo nano /etc/docker/daemon.json
```

```
{
    "registry-mirrors": [
        "https://docker.1panel.live",
        "https://hub.rat.dev"
    ]
}
```

##### 重启docker

```
sudo systemctl restart docker
```

##### 查看是否应用

```
sudo docker info
```

##### 拉取并创建容器mysql 5.7.27   root_password自己指定，和后面DzzOffice安装的时候保持一致

```
sudo docker pull mysql:5.7.27
sudo docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=数据库密码 mysql:5.7.27
```

##### 获取onlyoffice镜像:    JWT_SECRET=设置你的密钥    JWT_ENABLED=false  关闭

```
sudo docker pull onlyoffice/documentserver:7.3
sudo docker run -i -t -d -p 9001:80 -v /home/myOnlyOffice:/var/www/onlyoffice/documentserver/web-apps/wsData --env JWT_SECRET=abcdefghijklmnopqrstuvwxyz123456 -e JWT_ENABLED=false onlyoffice/documentserver:7.3
```



##### 获取Dzzoffice镜像:

```
sudo docker pull imdevops/dzzoffice
sudo docker run -d --name dzzoffice -p 80:80 imdevops/dzzoffice:latest    # 创建容器
```

##### 配置局域网代理  ip为主机ip

```
echo 'export ALL_PROXY=socks5://192.168.0.101:10810' >> ~/.bashrc

source ~/.bashrc
```



#### docker的命令

##### 启动docker
```
systemctl start docker
```



##### 关闭docker
```
systemctl stop docker
```



##### 重启docker
```
systemctl restart docker
```



##### docker设置随服务启动而自启动
```
systemctl enable docker
```

#### #-------------------------------------docker状态

##### 查看docker 运行状态
```
systemctl status docker
```



##### 查看docker 版本号信息
```
docker version
docker info
```

#### #-------------------------------------docker帮助

##### 忘记某些命令时，进行查看
```
docker --help
```

#如果忘记了 run命令 不知道可以带哪些参数 可以这样使用

```
docker run --help
```

#-------------------------------------镜像（增）
##### 拉取镜像（增）
##### 不加tag(版本号) 即拉取docker仓库中 该镜像的最新版本latest 加:tag 则是拉取指定版本
##### https://hub.docker.com/search?type=image （去官网镜像搜索）
```
docker pull 镜像名 
docker pull 镜像名:tag
```

#-------------------------------------镜像（查）
##### 查看镜像列表（查）
```
docker images
```




##### 搜索镜像（查）
```
docker search 镜像名
docker search --filter=STARS=9000 mysql 搜索 STARS >9000的 mysql 镜像
```

#-------------------------------------镜像（删）
##### 删除镜像（删）
##### 删除一个
```
docker rm -f 镜像名/镜像ID
```



##### 删除多个 其镜像ID或镜像用用空格隔开即可 
```
docker rm -f 镜像名/镜像ID 镜像名/镜像ID 镜像名/镜像ID
```



##### 删除全部镜像  -a 意思为显示全部, -q 意思为只显示ID
```
docker rmi -f $(docker images -aq)
```



##### 强制删除镜像
```
docker image rm 镜像名称/镜像ID
```

##### 删除未使用的资源，包括停止的容器、未使用的网络和构建缓存。这将提示你确认要删除的内容：

```bash
sudo docker system prune
```

##### #-------------------------------------镜像（存）

##### 保存镜像（存）
```
docker save 镜像名/镜像ID -o 镜像保存在哪个位置与名字
```


##### 示例
```
docker save tomcat -o /myimg.tar
```



##### 加载镜像（增）
```
docker load -i 镜像保存文件位置
```


##### 示例
```
docker load -i myimg.tar
```



##### 查看所有容器列表（包含 正在运行 和 已停止的）
```
docker ps -a
```



##### 停止容器
```
docker stop 容器ID/容器名
```



##### 重启容器
```
docker restart 容器ID/容器名
```



##### 启动容器
```
docker start 容器ID/容器名
```



##### kill 容器
```
docker kill 容器ID/容器名
```



##### ----------------容器文件拷贝 （无论容器是否开启 都可以进行拷贝）

##### docker cp 容器ID/名称:文件路径  要拷贝到外部的路径 | 要拷贝到外部的路径  容器ID/名称:文件路径

##### 从容器内 拷出
```
docker cp 容器ID/名称: 容器内路径  容器外路径
```



##### 示例：
```
docker cp nginx:/etc/nginx/conf.d /data/applications/nginx/conf/conf.d
```



##### 从外部 拷贝文件到容器内
```
docker  cp 容器外路径 容器ID/名称: 容器内路径
```



##### ----------------查看容器日志
```
docker logs -f --tail=要查看末尾多少行 默认all 容器ID
```



##### 示例：
```
docker logs -f -t --tail 1000 2ab447816a66
```



##### ----------------更换容器名
```
docker rename 容器ID/容器名 新容器名
```



##### 运行一个容器
##### -restart=always 该容器随docker服务启动而自动启动

```
docker run -it -d --name 要取的别名 镜像名:Tag /bin/bash 
```

命令参数说明：

```
-d：后台运行容器

-p：端口映射，格式为主机端口:容器端口

-e：设置环境变量，这里设置的是root密码

--name：设置容器别名

-v 挂载文件，格式为：宿主机绝对路径目录:容器内目录，

比如我们使用：-v /usr/local/mysql/logs:/var/log/mysql

将mysql容器存放日志文件的目录：/var/log/mysql挂载在宿主机的/usr/local/mysql/logs下
```


##### 示例
```
docker run --name mysql \
-v /myapp/mysql:/var/lib/mysql \
-p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:8.0.19
```



##### 停止运行的 redis 容器 
```
docker stop 容器名/容器ID
```

##### #删除一个容器

```
docker rm -f 容器名/容器ID
```

##### #删除多个容器 空格隔开要删除的容器名或容器ID

```
docker rm -f 容器名/容器ID 容器名/容器ID 容器名/容器ID
```

##### #删除全部容器

```
docker rm -f $(docker ps -aq)
```

##### #进入容器(方式一)

```
docker exec -it 容器名/容器ID /bin/bash
```

##### #进入容器(方式二) --- 不推荐使用

```
docker attach 容器名/容器ID
```



 ##### 直接退出 （如果没有添加-d 参数(持久化运行容器) 该容器会被关闭 ） 
```
exit
```



##### 优雅退出 （无论是否添加-d 参数 容器都不会被关闭）
```
Ctrl + p + q
```

#### #------------------------------防火墙命令

##### // 1.检验防火墙是否启动

```
firewall-cmd --state
```

##### // 2. 检查端口是否启动：

```
firewall-cmd --permanent --zone=public --list-ports
```

##### //3.开启 8080 端口：

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
```

##### //4.重新启动防火墙

```
firewall-cmd --reload
```

##### #永久关闭防火墙,和SElinux

```
systemctl stop firewalld
systemctl disable firewalld

setenforce 0
sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
```



##### 构建一个新的镜像
```
docker commit -m="提交信息" -a="作者信息" 容器名/容器ID 提交后的镜像名:Tag
```



##### 查看docker磁盘占用总体情况
```
du -hs /var/lib/docker/ 
```



##### 查看Docker的磁盘使用具体情况
```
docker system df
```



##### ----------------------------删除 无用的容器和 镜像
#####  删除异常停止的容器
```
docker rm `docker ps -a | grep Exited | awk '{print $1}'` 
```



#####  删除名称或标签为none的镜像
```
docker rmi -f  `docker images | grep '<none>' | awk '{print $3}'`
```



#####  清除所有无容器使用的镜像 (只要是镜像无容器使用（容器正常运行）都会被删除，包括容器临时停止)
```
docker system prune -a
```




##### ----------------------------查找大文件
```
find / -type f -size +100M -print0 | xargs -0 du -h | sort -nr
```



##### 查找指定docker使用目录下大于指定大小文件
```
find / -type f -size +100M -print0 | xargs -0 du -h | sort -nr |grep '/var/lib/docker/overlay2/*'
```



##### 构建镜像 （需要在Dockerfile同级目录下构建）
```
docker build -t cat:1.0 .
```



##### 说明（-t：设置 镜像的名字及tag）（最后的. 为当前目录）

```
FROM
基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，第一条必须是from

MAINTAINER
镜像维护者的姓名和邮箱地址
```

```
RUN
```

##### 容器构建时需要运行的命令

两种格式

shell格式（1）

```
RUN yum -y install vim
```

exec格式（2）

```
["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

##### 注意：RUN 是在 docker build时运行

```
EXPOSE
```

##### 当前容器对外暴露出的端口

```
WORKDIR
```

##### 指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

```
USER
```

##### 指定该镜像以什么样的用户去执行，如果都不指定，默认是root

```
ENV
```

##### 用来在构建镜像过程中设置环境变量

```
ENV MY_PATH /usr/mytest
```

##### 这个环境变量可以在后续的任何RUN指令中使用，这就如同在命令前面指定了环境变量前缀一样；

##### 也可以在其它指令中直接使用这些环境变量，

比如：

```
WORKDIR $MY_PATH
```

```
ADD
```

##### 将宿主机目录下的文件拷贝进镜像且会自动处理URL和解压tar压缩包

```
COPY
```

##### 类似ADD，拷贝文件和目录到镜像中。 

##### 将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置

```
COPY src dest
COPY ["src", "dest"]
```

<src源路径>：源文件或者源目录
<dest目标路径>：容器内的指定路径，该路径不用事先建好，路径不存在的话，会自动创建。

```
VOLUME
```

##### 容器数据卷，用于数据保存和持久化工作

```
CMD
```

指定容器启动后的要干的事情

注意：Dockerfile 中可以有多个 CMD 指令，
但只有最后一个生效，CMD 会被 docker run 之后的参数替换

Compose 是 Docker 公司推出的一个工具软件，可以管理多个 Docker 容器组成一个应用。你需要定义一个 YAML 格式的配置文件docker-compose.yml，写好多个容器之间的调用关系。然后，只要一个命令，就能同时启动/关闭这些容器。

简而言之：

Docker-Compose是Docker官方的开源项目， 负责实现对Docker容器集群的快速编排。


```
docker-compose -h                           # 查看帮助

docker-compose up                           # 启动所有docker-compose服务
docker-compose up -d                        # 启动所有docker-compose服务并后台运行

docker-compose down                         # 停止并删除容器、网络、卷、镜像
docker-compose exec  yml里面的服务id         # 进入容器实例内部yml文件中写的服务id /bin/bash

docker-compose ps                      # 展示当前docker-compose编排过的运行的所有容器
docker-compose top                     # 展示当前docker-compose编排过的容器进程

docker-compose logs  yml里面的服务id     # 查看容器输出日志

docker-compose config     # 检查配置
docker-compose config -q  # 检查配置，有问题才有输出

docker-compose restart   # 重启服务
docker-compose start     # 启动服务
docker-compose stop      # 停止服务


```

