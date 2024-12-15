---
title: Github上传教程
---



1，项目根目录，鼠标右键，使用Git Bash Here 打开

2，初始化，在本地创建一个Git仓库

```
git init
```

3，输入以下命令，将项目添加到暂存区

```
git add .
```

注意： **.** 前面有空格，代表添加所有文件。若添加单个文件输入：`git add xxxx.xx`。

4，输入以下命令，将项目提交到Git仓库

```
git commit -m "注释内容"
```

5，输入 以下命令，上传到 main 分支

```
git branch -M main
```

`6， 输入以下命令和远程仓库连接

```
git remote add origin https://github.com/xxxx/xxx.git
```

使用以下命令查看当前配置的远程仓库 URL：

```
git remote -v
```

注意：xxxx为自己的GitHub名，xxx 为仓库名。

7,输入以下命令将本地项目推送到远程仓库

```
git push -u origin main
```

8，如何解决md文档图片上传仓库不显示，上传图片至仓库后，复制图片地址，替换md文件的图片地址，将图片链接中的blob替换成raw即可正常显示。