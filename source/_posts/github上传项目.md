---
title: Github上传教程
---



##### 1，项目根目录，鼠标右键，使用Git Bash Here 打开

##### 2，初始化，在本地创建一个Git仓库

```
git init
```

##### 3，输入以下命令，将项目添加到暂存区

```
git add .
```

注意： **.** 前面有空格，代表添加所有文件。若添加单个文件输入：`git add xxxx.xx`。

##### 4，输入以下命令，将项目提交到Git仓库

```
git commit -m "update"
```

##### 5，输入 以下命令，上传到 main 分支

```
git branch -M main
```

##### `6， 输入以下命令和远程仓库连接

```
git remote add origin https://github.com/xxxx/xxx.git
```

##### 使用以下命令查看当前配置的远程仓库 URL：

```
git remote -v
```

注意：xxxx为自己的GitHub名，xxx 为仓库名。

##### 7,输入以下命令将本地项目推送到远程仓库

```
git push -u origin main     --force  强制推送本地分支，这会覆盖远程分支上的更改
```

##### 如果推送失败，可以改用ssh推送

##### 生成ssh密钥

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

`your_email@example.com`替换为你的电子邮件地址。这个命令会生成一个RSA密钥对，并存储在默认位置（`~/.ssh/id_rsa`和`~/.ssh/id_rsa.pub`）

##### 添加SSH公钥到远程仓库

将生成的公钥（`id_rsa.pub`）添加到你的远程仓库（如GitHub）：

- 打开`~/.ssh/id_rsa.pub`文件，复制其内容。
- 登录到你的GitHub账户。
- 点击右上角的头像，选择“Settings”。
- 在左侧菜单中选择“SSH and GPG keys”。
- 点击“New SSH key”按钮。
- 在“Title”字段中，可以输入一个描述（例如，你的计算机名称）。
- 在“Key”字段中，粘贴你的公钥内容。
- 点击“Add SSH key”按钮。

##### 全局配置Git使用SSH

```
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

##### 现在，当你克隆或推送代码时，Git会使用SSH而不是HTTPS。例如，克隆仓库：

```bash
git clone git@github.com:username/repo.git
```

##### 推送代码：

```bash
git push origin main
```