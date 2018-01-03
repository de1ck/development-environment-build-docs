## 提供项：

## git 服务器 [IP]（[IP]）、仓库名（[reponame]） 、Email（[yourEmail]）

```shell
# 安装 Git
git --version 查看 Git 版本信息

# 初始化仓库 (统一在git服务器 /home/gitrepo/ 目录下)
git init --bare [reponame].git

# 以上命令 Git 创建一个空仓库，服务器上的 Git 仓库通常都以.git 结尾。然后，把仓库所属用户改为 git：
chown -R git:git [reponame].git
```

## 客户端创建 SSH 公钥和私钥 (ssh 公钥/私钥登陆)

```shell
在git bash 里面运行以下命令
ssh-keygen -t rsa -C "[yourEmail]" （全部 enter 通过）

# 此时 C:\Users\用户名\.ssh 下会多出两个文件 id_rsa 和 id_rsa.pub
id_rsa 是私钥
id_rsa.pub 是公钥
```

## <span id="import-key">将客户端公钥导入服务器端 /home/git/.ssh/authorized_keys 文件</span>

```shell
ssh git@[IP] 'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub （或者 ~/.ssh/[name].pub）
 # 输入 git 用户密码

# 测试 ssh 登录 git 用户 (使用 SSH 公钥、私钥免密码登录)
ssh -T git@[IP]

# 查看密钥 使用 Root 用户 git 用户已禁止 ssh 登录服务器
vi /home/git/.ssh/authorized_keys

# 克隆仓库
git clone git@[IP]:/home/gitrepo/[reponame].git

# 开始愉快地使用 git 命令了
```

## 多 git 服务器密钥管理 [name]

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

### 然后 将客户端公钥导入服务器端 /home/git/.ssh/authorized_keys 文件 注意修改 ~/ssh 后面的文件 [点击跳转](#import-key)

## 参考链接： https://www.cnblogs.com/dee0912/p/5815267.html
