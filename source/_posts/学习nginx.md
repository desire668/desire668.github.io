---

title: 学习centos7.4 nginx
---
下载centos7.4 nimimal

[centos-vault-7.4.1708-isos-x86_64安装包下载_开源镜像站-阿里云](https://mirrors.aliyun.com/centos-vault/7.4.1708/isos/x86_64/)

ip addr 查看网卡名称

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

手动指定ip

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static    #dhcp改成static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=bddb4623-b6be-4df6-b57b-488e55517b99
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.0.102   #添加ip
NETMASK=255.255.255.0	#添加子网掩码
GATEWAY=192.168.0.1		#添加网关
DNS1=8.8.8.8			#添加dns
DNS2=114.114.114.114
```

重启网卡

```
systemctl restart network
```

yum换源

```
vi /etc/yum.repos.d/CentOS-Base.repo 
```



```
# CentOS-Base.repo

#

# The mirror system uses the connecting IP address of the client and the

# update status of each mirror to pick mirrors that are updated to and

# geographically close to the client.  You should use this for CentOS updates

# unless you are manually picking other mirrors.

#

# If the mirrorlist= does not work for you, as a fall back you can try the 

# remarked out baseurl= line instead.

#
#

[base]
name=CentOS-$releasever - Base - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/
        http://mirrors.aliyuncs.com/centos/$releasever/os/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

#released updates 
[updates]
name=CentOS-$releasever - Updates - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos/$releasever/updates/$basearch/
        http://mirrors.aliyuncs.com/centos/$releasever/updates/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos/$releasever/extras/$basearch/
        http://mirrors.aliyuncs.com/centos/$releasever/extras/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos/$releasever/centosplus/$basearch/
        http://mirrors.aliyuncs.com/centos/$releasever/centosplus/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos/$releasever/contrib/$basearch/
        http://mirrors.aliyuncs.com/centos/$releasever/contrib/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
```

```bash
yum clean all
```

```bash
yum makecache
```

nginx开源版本安装1.21

[nginx: download](https://nginx.org/en/download.html)

上传到/root/目录下,解压

```
tar -zxvf nginx-1.22.1.tar.gz 
```

```
cd nginx-1.22.1
```

```
./configure --prefix=/usr/local/nginx
```

如果提示以下报错

```
checking for OS

 + Linux 3.10.0-693.el7.x86_64 x86_64
   checking for C compiler ... not found

./configure: error: C compiler cc is not found
```

安装c语言解释器

```
yum install -y gcc 
```

如果提示以下报错

```
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```

安装pcre

```
yum install -y pcre pcre-devel
```

如果提示以下报错

```
./configure: error: the HTTP gzip module requires the zlib library.
You can either disable the module by using --without-http_gzip_module
option, or install the zlib library into the system, or build the zlib library
statically from the source with nginx by using --with-zlib=<path> option.
```

安装zlib

```
yum install -y zlib zlib-devel
```



```
make
make install
```

进入nginx安装目录

```
cd /usr/local/nginx/sbin
```

运行nginx

```
./ nginx 启动
./ nginx -s stop 快速停止
./ nginx -s quit 优雅停止，推出前完成已经接受的连接请求
./ nginx -s reload  重新加载配置
```

关闭防火墙

```
systemctl stop firewalld.service
```

关闭防火墙开机自启

```
systemctl disable firewalld.service
```

放行80端口

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

重启防火墙

```
systemctl restart firewalld.service
```

安装成系统服务

```
vi /etc/systemd/system/nginx.service
```



```
[Unit]
Description=nginx - web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

重新加载系统服务

```
systemctl daemon-reload
```

停止nginx

```
./nginx -s quit
```

查看nginx进程是否还在

```
ps -ef | grep nginx
```

启动nginx

```
systemctl start nginx.service
```

开机启动

```
systemctl enable nginx.service
```

最小化解释 nginx配置文件

```
vi /usr/local/nginx/conf/nginx.conf  
```



```
worker_processes  1;    #一个核心对应一个进程

events {
    worker_connections  1024;  #一个进程对应多少连接
}


http {
    include       mime.types;     #引入配置文件，mime.types 告诉浏览器是什么文件
    default_type  application/octet-stream;  #默认以数据流格式传输到客户端


    sendfile        on;    #减少nginx复制读取的过程



    keepalive_timeout  65;  #超时连接


#虚拟主机  
    server {
        listen       80;	#监听端口
        server_name  localhost; #域名，主机名 一个server_name可以指定多个域名，完整匹配，通配符匹配，通配符结束匹配，正则匹配

#location指的是某一段的uri
        location / {
#相对路径
            root   html;
            index  index.html index.htm; #默认页
        }
#错误页面
 		error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

}
```

nginx是隧道式代理，受本身带宽限制，lvs是专业的负载均衡器（dr模型）
