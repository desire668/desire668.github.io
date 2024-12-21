---
title: docker快速入门

---

[开始使用 -- Docker官方文档|Docker中文文档|Docker中文文档|Docker官方教程](https://docker.cadn.net.cn/)

#### docker file 

```
FROM python:3.8-slim-buster	 #官方镜像名字:版本号      

WORKDIR  /app        #未存在docker会自动创建   

COPY . .   	# <本地路径><目标路径>  "."代表程序根目录 下所有文件  ，第二个参数代表docker镜像中的路径

RUN pip3 install -r requirements.txt    #运行shell命令  

CMD ["python3","app.py"]  #docker运行起来以后运行的命令   

```

##### 构建docker容器

```
docker build -t my-docker  .   	#-t 指定镜像名字   .  当前路径查找docker file
```

##### 运行docker容器

```
docker run -p	80:5000	-d	my-docker -v my-docker-data:/etc/docker	
#	-p 将容器的端口映射到主机的端口 ,前面是主机的端口，后面是容器的端口  
	-d 让容器在后台运行 
	-v 指定将数据卷挂载到容器中的那一个路径上，向那个数据写入的任何数据都会保存到数据卷中
  
```

##### 显示所有容器

```
docker ps
```

##### 停止容器

```
docker stop <容器id>
```

##### 重启容器

```
docker restart <容器id>
```

##### 删除容器

```
docker rm <容器id>
```

##### 启动一个远程shell

```
docker exec -it <容器id> /bin/bash
```

##### 创建一个数据卷

```
docker volume create my-docker-data 
```

docker-compose