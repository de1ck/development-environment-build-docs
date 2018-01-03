#Node.js 安装、配置 windows 版

## 安装软件

淘宝镜像 8.9.3 (请安装 8.x 版本)

[nodejs .msi \*64](https://npm.taobao.org/mirrors/node/v8.9.3/node-v8.9.3-x64.msi)

[nodejs .zip \*64](https://npm.taobao.org/mirrors/node/v8.9.3/node-v8.9.3-win-x64.zip)

---

## 配置

### global 与 cache (nodeePath 为 nodejs 绝对路径)

在 nodejs 安装目录或解压目录下 新建 node_global 和 node_cache 两个文件夹

```shell
npm config set prefix "nodePath\node_global"
npm config set cache "nodeePath\node_cache"
#查看效果
npm config list
```

### 环境变量配置

1.解压版 zip

1.1 使用系统变量 系统变量下新建

NODE_HOME=nodeePath

NODE_PREFIX=nodeePath\node_global

NODE_PATH="nodeePath\node_global\node_modules"

1.2 加入 Path

在 Path 中添加 %NODE_HOME%;%NODE_PREFIX%

2.安装版 msi

安装可能会默认把 NODE_HOME 变量加入 Path 中 其他不变

### 查看版本信息（使用安装版 || 或 解压版配置好系统变量后使用） ( Nodejs 自带 npm 包管理器)

```shell
 node -v

 npm -v
```

### 淘宝镜像

```shell
# 查看 npm 配置
npm config ls(list)

# 配置淘宝镜像
npm config set registry https://registry.npm.taobao.org
npm install -g cnpm --registry=https://registry.npm.taobao.org

# 使用 cnpm
cnpm i [name]
```

## 包管理器 （Npm VS Yarn）

yarn 它解决了 npm 的一些缺陷 推荐使用 yarn 替代 npm 进行包管理

[Yarn doc](https://yarnpkg.com/zh-Hans/docs/cli/)

```shell
# 全局安装
cnpm i -g yarn

# 查看版本
yarn -v

# 查看配置
yarn config list

# 配置淘宝镜像
yarn config set registry https://registry.npm.taobao.org
```
