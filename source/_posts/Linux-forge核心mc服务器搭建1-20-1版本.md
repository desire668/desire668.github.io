---
title: Linux forge核心mc服务器搭建1.20.1版本
date: 2024-12-02 23:45:55
tags:
---

### 1：浏览器搜索阿里云，首页点击产品，再点击云服务器ECS进去，旁边有个免费试用，点击进去，（如果没有试用就点击购买）配置选择2核心4G，系统选择乌班图Ubuntu，然后设置一下登录凭证密码

[![img](https://desire668.serv00.net/wp-content/uploads/2024/06/图片2-1024x378.png)](https://desire668.serv00.net/wp-content/uploads/2024/06/图片2.png)

### 2：右上角点击控制台，点击刚刚购买的云服务器，再点击左边网络与安全的安全组，创建一个新的安全组，点击管理规则，出入放行一下25565这个端口。

[![img](https://desire668.serv00.net/wp-content/uploads/2024/06/图片3-1024x526.png)](https://desire668.serv00.net/wp-content/uploads/2024/06/图片3.png)

[![img](https://desire668.serv00.net/wp-content/uploads/2024/06/图片4-1024x219.png)](https://desire668.serv00.net/wp-content/uploads/2024/06/图片4.png)

### 3：下载一个finalshell之类的远程工具，浏览器直接搜或者在我群可以下载到。点击左上角文件夹图标新建一个ssh链接，名称随便填，主机地址填你云服务器的公网ip，用户和密码填你设置的密码，点击应用就链接上了。

[![img](https://desire668.serv00.net/wp-content/uploads/2024/06/图片5.png)](https://desire668.serv00.net/wp-content/uploads/2024/06/图片5.png)

### 4.更新列表

```
sudo apt update
```

###   （centos得话把apt换成yum就行）

### 5. 安装java

```
sudo apt install default-jdk
```

### 

### 6.查看java版本，因为1.20.1版本需要高版本java，如果java版本是低版本如Java11，需要自行百度下载java17或18，手动安装不然会报错

```
java -version
```



### 7.然后点击home文件夹，在右边空白区域右键创建一个mc文件夹，点击进去，服务器文件直接拿拖进去，然后输入以下命令 进入

```
cd /home/mc
```



### 8.输入以下命令运行服务器安装

```
java -jar forge-1.20.1-47.2.0-installer.jar nogui --installServer
```

####  运行服务器文件会多出一个文件，双击打开user_jvm_args.txt，如果你也是2核4g的服务器就添加以下代码，意思是最小内存和最大内存

```
#### -Xms1024M

#### -Xmx3072G
```



### 9.运行服务器

```
bash run.sh
```

### 等待运行完后会出现eula.txt这个文件，双击进去把false 改成true

#### 再次输入运行

```
bash run.sh 
```

#### 输入stop停止服务器 刷新一下会出现server.properties，双击打开，找到online-mode=false，把true改成false，这个是正版验证的意思

### 10. 安装screen

```
sudo apt install screen
```

### 11.

```
screen -S mc # 创建一个窗口名字叫mc的窗口
```



### 12.再次输入运行bash run.sh，服务器就启动了，后续如果要添加mod，把对应版本核心的mod文件就直接拖到mod文件夹里面即可

### 13.接下来进入游戏，点击多人游戏输入服务器公网ip加端口即可与小伙伴们畅玩

[![img](https://desire668.serv00.net/wp-content/uploads/2024/06/图片6-1024x616.png)](https://desire668.serv00.net/wp-content/uploads/2024/06/图片6.png)

```
screen -d -r name # 回到名字为name的窗口

screen -X -S name quit   # 关闭名字为name的窗口
```

