---
title: Github+Hexo搭建博客
date: 2024-12-03 01:25:55
tags:
---



### 1：创建github账号，并登陆

 

### 2：在你github主页左上角有个repositories，点击旁边的new，新建仓库

 

### 3:填你的github用户名加github.io



 

### 4：回到主页，进入新创建的库，点击setting



### 5：点击pages再点击 github pages，返回是个网页。



 

### 6：下载node.js安装  node -v 查看版本是否 安装。下载git安装，git -v 查看版本是否安装

 

### 7：新建一个文件夹，在空白处右键 open git bash here 输入以下两行命令，双引号区域替换成你的github账户和邮箱

 

```
git config --global user.name"desire668"

git config --global [user.emal"2082216455@qq.com](mailto:user.emal\)
```

 

### 6:打开终端输入以下命令，安装hexo

```
npm install -g hexo-cli
```

### 7：在创建的文件夹下，打开 git bash here ，输入以下命令

```
hexo init myBlog     （ctrl+c退出）

cd myBlog 

npm install
```

 

### 7：若是上面的命令都没报错的话，就恭喜了，运行 hexo s 命令，在浏览 器中输入 http://localhost:4000 回车就能够预览效果了

### 8：自行去官网上找自己喜欢的主题下载 https://hexo.io/themes

### 9：打开你创建的文件夹\myBlog\themes,在空白处右键 open git bash here ,运行你喜欢的主题的安装命令

### 10：_config.yml 文件中修改theme：（此为主题名）

### 11：保存配置文件后，运行以下三条命令，重新生成静态文件并启动服务，在浏览器中输入http://localhost:4000回车就能够预览修改主题后的效果了

 

```
hexo clean

hexo g

hexo s
```

### 12：主题具体怎么配置自己看说文档，接下来配置git，输入以下命令

#### \# 以下 user.name 和 user.email 输入自己的

```
git config --global user.name "这"

git config --global user.email 这
```

### 13：使用 ssh-keygen 生成私钥和公钥

```
ssh-keygen -t rsa
```

#### 一路回车键，记住你的公钥 id_rsa.pub 的位置。

### 14：接着在 GitHub 头像下的 Settings 里找到添加 SSH key，点击New SSH key 。将刚刚生成的公钥 id_rsa.pub 文件里的内容复制到 Key 里面（用 记事本 打开公钥文件），然后选择添加，GitHub 会提示输入密码确认。

### 15：终端输入 

```
ssh -T [git@github.com](mailto:git@github.com)
```

####  第一次的时候会让你 yes 确认 ，如果看到 Hi 后面是自己的用户名，就说明成功了

### 16：回到项目文件夹，编辑hexo 的配置文件 _config.yml	最下面

```
deploy:

 type: git

 repository: https://github.com/用户名/用户名.github.io.git

 branch: main
```

### 17:安装 Git 插件

```
 npm install hexo-deployer-git --save
```

### 18：部署

```
 hexo c && hexo g && hexo d 
```

### 19:访问 

你的用户名.github.io

![image-20241203012717242](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20241203012717242.png)

####  如何将hexo源码也备份到github上，并且与博客互不冲突

##### 首先确保自己已经使用hexo在github搭建好了自己的个人博客

 在Github的username.github.io仓库上新建一个xxx分支，并切换到该分支，并在该仓库->Settings->Branches->Default branch中将默认分支设为xxx，save保存；然后将该仓库克隆到本地，进入该username.github.io文件目录。

完成上面步骤后，在当前目录使用Git Bash执行`git branch`命令查看当前所在分支，应为新建的分支xxx：

先将本地博客的部署文件（**Hexo目录下的全部文件**）全部拷贝进username.github.io文件目录中去。

接下来，进入username.github.io文件目录下，将该目录下的全部文件提交到xxx分支，提交之前需注意：

> - 将themes目录以内中的主题的.git目录删除（如果有），因为一个git仓库中不能包含另一个git仓库，提交主题文件夹会失败。
> - 可能有人会问，删除了themes目录中的.git不就不能`git pull`更新主题了吗，很简单，需要更新主题时在另一个地方`git clone`下来该主题的最新版本，然后将内容拷到当前主题目录即可

#### 提交hexo分支

执行

```
git add .

git commit -m 'up hexo files

git push
```

即可将博客的hexo部署环境提交到GitHub个人仓库的xxx分支。

master分支和xxx分支各自保存着一个版本，master分支用于保存博客静态资源，提供博客页面供人访问；xxx分支用于备份博客部署文件，供自己维护更新，两者在一个GitHub仓库内互不冲突，完美！

编辑、撰写文章或其他博客更新改动

依次执行`git add .`、`git commit -m 'back up hexo files'`（引号内容可改）、`git push`指令，保证xxx分支版本最新

执行`hexo d -g`指令（在此之前，有时可能需要执行`hexo clean`），完成后就会发现，最新改动已经更新到master分支了，两个分支互不干扰！


参考链接：https://www.jianshu.com/p/0b1fccce74e0



 