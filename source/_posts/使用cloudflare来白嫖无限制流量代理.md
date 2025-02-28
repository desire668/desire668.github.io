---
title: 使用cloudflare来白嫖无限制流量代理
---

1.首先打开cloudflare网站注册账号并登陆

```
https://dash.cloudflare.com/
```

2.从这个github仓库下载脚本

https://github.com/cmliu/edgetunnel?tab=readme-ov-file 

3.打开cloudflare左侧栏，计算=> work和pages，点击pages，再点击上传，从github仓库下载的文件。点保存部署。

4.点击部署好的pages，点击设置，新增环境变量，名称为uuid，值得话填写从以下网址获取得uuid

```
https://free.xwteam.cn/doc/168   
```

5。点击pages的部署，重新上传从github仓库下载的文件。

6.订阅地址为你的pages地址/uuid地址，即可享用。