---
title: centos 9 部署本地gitlab
---



##### 下载 Gitlab

```
mkdir /usr/local/gitlab
```

##### 进入创建的目录

```
cd /usr/local/gitlab
```

##### 下载 Gitlab 安装包，wget下载速度慢，可以手动下载再上传到目录下

```
wget --content-disposition https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/8/gitlab-ce-15.0.2-ce.0.el8.x86_64.rpm/download.rpm
```

##### 下载成功后,首先安装一个工具包 ，下载慢的话可以换个阿里源

```
yum install  policycoreutils-python-utils
```

##### 在原来创建的目录运行安装命令

```
rpm -Uvh gitlab-ce-15.0.2-ce.0.el8.x86_64.rpm
```

##### 安装完成后，更新配置

```
gitlab-ctl reconfigure
```

##### 等待更新完，会提供用户名以及password路径

```
cat /etc/gitlab/initial_root_password
```

##### 启动gitlab

```
gitlab-ctl start
```

##### 修改访问地址

```
vim /etc/gitlab/gitlab.rb
```

本地的话就把 external_url 的值换成http://本地ip:8888   端口可以自己指定

##### 修改完重新加载配置文件

```
gitlab-ctl reconfigure
```

##### 记得防火墙放行端口

```
firewall-cmd --zone=public --add-port=8088/tcp --permanent
```

##### 重新启动 Gitlab

```
gitlab-ctl restart
```

##### 然后打开浏览器,输入本地 ip+8088,即可访问成功，如果无法访问，关闭防火墙即可

```
systemctl stop firewalld
```

##### 停止gitlab所有服务

```bash
gitlab-ctl stop
```
