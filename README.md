# gitrepo-readme

参考链接： https://www.cnblogs.com/dee0912/p/5815267.html

[nodejs 篇](https://github.com/de1ck/software-use-docs/blob/master/docs/nodejs.md)

提供项：

git 服务器 IP（IP）、仓库名（reponame） 、Email（yourEmail）

## 安装 Git

git --version 查看 Git 版本信息

## 初始化仓库 (统一在 /home/gitrepo/ 目录下)

git init --bare reponame.git

以上命令 Git 创建一个空仓库，服务器上的 Git 仓库通常都以.git 结尾。然后，把仓库所属用户改为 git：

chown -R git:git reponame.git

## 客户端创建 SSH 公钥和私钥

ssh-keygen -t rsa -C "yourEmail" （全部 enter 通过）

此时 C:\Users\用户名\.ssh 下会多出两个文件 id_rsa 和 id_rsa.pub
id_rsa 是私钥

id_rsa.pub 是公钥

## 将客户端公钥导入服务器端 /home/git/.ssh/authorized_keys 文件

ssh git@IP 'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub

## 查看 密钥

ssh git@IP 输入 git 用户密码

vi /home/git/.ssh/authorized_keys

## 克隆仓库

git clone git@IP:/home/gitrepo/reponame.git
