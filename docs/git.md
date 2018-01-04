# 提供项：

## git 服务器 IP（[IP]）、仓库名（[reponame]） 、Email（[yourEmail]）、用户名([yourName])

## [搭建 Git 服务器](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)

```bash
# 第一步 安装git
apt-get install git

# 第二步 创建一个git用户，用来运行git服务
adduser git
```

### 第三步 [客户端创建证书登录](#客户端创建证书登录)

### 收集所有需要登录的用户的公钥，就是他们自己的 id_rsa.pub 文件，把所有公钥导入到/home/git/.ssh/authorized_keys 文件里，一行一个

```bash
# 第四步，初始化Git仓库：
# 先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：
git init --bare sample.git

# 把owner改为git
chown -R git:git sample.git

# 第五步，禁用shell登录：
vi /etc/passwd

# 找到类似下面的一行：
git:x:1001:1001:,,,:/home/git:/bin/bash
#改为：
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

# 第六步，客户端克隆远程仓库：
git clone git@server:/srv/sample.git
```

## 安装客户端 [Git](https://git-scm.com/)

```shell
# 查看 Git 版本信息
git --version
```

ps1: 以下命令请在 git bash 终端中使用 (鼠标右键进入)

## [初次运行 Git 前的配置](https://git-scm.com/book/zh/v1/起步-初次运行-Git-前的配置)

```shell
# 设置 用户名 邮件地址 进入 git bash 命令
git config --global user.name [yourName]
git config --global user.name [yourEmail]

# 查看全局 配置列表
git config --global --list

# 手动修改全局配置 .gitconfig文件
vi ~/.gitconfig
```

## 客户端创建证书登录

```shell
# 在git bash 里面运行以下命令
ssh-keygen -t rsa -C "[yourEmail]" （全部 enter 通过）
# 此时 C:\Users\用户名\.ssh 下会多出两个文件 id_rsa 和 id_rsa.pub
id_rsa 是私钥
id_rsa.pub 是公钥

# 查看公钥
vi ~/.ssh/id_rsa.pub
```

## 将客户端公钥导入服务器端

### 默认在服务器 git 用户目录下 /home/git/.ssh/authorized_keys 文件

```shell
ssh git@[IP] 'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub （或者 ~/.ssh/[name].pub）
# 输入 git 用户密码

# 测试 ssh 登录 git 用户 (使用 SSH 公钥、私钥免密码登录)
ssh -T git@[IP]
# 如果不需要输入git用户密码表示密钥导入成功
# 如果失败，请直接使用root用户编辑authorized_keys 文件 (git用户已禁止ssh登录服务器)
vi  /home/git/.ssh/authorized_keys
# 将  ~/.ssh/[name].pub 文件内容粘贴在文件末尾新的一行中(每个用户密钥另起一行)

# 可以用root用户查看系统日志 查找错误原因
tail /var/log/secure -n 20


# 克隆仓库
git clone git@[IP]:/home/gitrepo/[reponame].git

# 开始愉快地使用 git 命令了
```

## 多 git 服务器密钥管理

1 生成 ssh 公钥 私钥

```shell
# 在 git bash 里面运行以下命令
ssh-keygen -t rsa  -C "[yourEmail]"  -f ~/.ssh/[name] （全部 enter 通过）

# win7会在C:\Users\Administrator\.ssh生成两个文件  [name].pub 和[name]两个文件

# 进入 .ssh 目录
cd ~/.ssh

# 新建 config 文件
touch config

vi config

## 编辑以下内容

host  [IP]

user git

hostname   [IP]

port 22

identityfile  C:/Users/Administrator/.ssh/[name]
```

2 [将客户端公钥导入服务器端](#将客户端公钥导入服务器端)

## 新建仓库

```shell
# 初始化仓库 (统一在 git 服务器 /home/gitrepo/ 目录下)
git init --bare [reponame].git

# 以上命令 Git 创建一个空仓库，服务器上的 Git 仓库通常都以.git 结尾。然后，把仓库所属用户改为 git：
chown -R git:git [reponame].git
```

## 参考链接：

[git 官网](https://git-scm.com/)

[搭建 Git 服务器](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)

[在 Linux 下搭建 Git 服务器](https://www.cnblogs.com/dee0912/p/5815267.html)

[git bash 使用 ssh 多服务器 公钥/私钥登陆 centos](http://blog.csdn.net/wql19881207/article/details/37387879)

[CentOS 配置 SSH 免密码登录后,仍提示输入密码](http://www.linuxidc.com/Linux/2014-10/107762.htm)
