# Commit message 和 Change log 编写指南

[git 提交信息指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

## Commit message 的格式

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。

```shell
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

### Header

Header 部分只有一行，包括三个字段：type（必需）、scope（可选）和 subject（必需）。

**（1）type**

type 用于说明 commit 的类别，只允许使用下面 7 个标识。

* feat：新功能（feature）
* fix：修补 bug
* docs：文档（documentation）
* style： 格式（不影响代码运行的变动）(空格，格式化，缺少分号等)
* refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
* perf: 改变代码，提高性能(performance)
* test：增加测试
* build: 影响构建系统或外部依赖项的更改(webpack,gulp,broccoli,npm)
* ci: 改变 CI 配置文件或者脚本（Travis,Circle,BrowserStack,SauceLabs）
* chore：构建过程或辅助工具的变动

**（2）scope**

scope 用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

**（3）subject**

subject 是 commit 目的的简短描述，不超过 50 个字符。

* 以动词开头，使用第一人称现在时，比如 change，而不是 changed 或 changes
* 第一个字母小写
* 结尾不加句号（.）

### Body

Body 部分是对本次 commit 的详细描述，可以分成多行。

### Footer

Footer 部分只用于两种情况。

**（1）不兼容变动**

如果当前代码与上一个版本不兼容，则 Footer 部分以 BREAKING CHANGE 开头，后面是对变动的描述、以及变动理由和迁移方法。

**（2）关闭 Issue**

如果当前 commit 针对某个 issue，那么可以在 Footer 部分关闭这个 issue 。

### Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以 revert:开头，后面跟着被撤销 Commit 的 Header。

```shell
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

Body 部分的格式是固定的，必须写成 This reverts commit &lt;hash>.，其中的 hash 是被撤销 commit 的 SHA 标识符。如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的 Reverts 小标题下面。

## Commitizen

Commitizen 是一个撰写合格 Commit message 的工具。

```bash
# 安装命令如下。
npm install -g commitizen

# 然后，在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式
commitizen init cz-conventional-changelog --save --save-exact
# 以后，凡是用到git commit命令，一律改为使用git cz。这时，就会出现选项，用来生成符合格式的 Commit message。
```

## validate-commit-msg

validate-commit-msg 用于检查 Node 项目的 Commit message 是否符合格式。

```bash
# 安装 ghooks validate-commit-msg
npm install ghooks validate-commit-msg --save-dev
# package.json里面使用 ghooks，把这个脚本加为commit-msg时运行。 https://www.npmjs.com/package/ghooks
"config": {
    "ghooks": {
      "commit-msg": "validate-commit-msg"
    }
  }
# 然后，每次git commit的时候，这个脚本就会自动检查 Commit message 是否合格。如果不合格，就会报错
$ git add -A
$ git commit -m "edit markdown"
INVALID COMMIT MSG: does not match "<type>(<scope>): <subject>" ! was: edit markdown

# 在 vscode 中可以使用 Commitizen 扩展来提交commit信息  https://marketplace.visualstudio.com/items?itemName=KnisterPeter.vscode-commitizen
```

## 生成 Change log

如果你的所有 Commit 都符合 Angular 格式，那么发布新版本时， Change log 就可以用脚本自动生成

生成的文档包括以下三个部分。

* New features
* Bug fixes
* Breaking changes.

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

这里我提供 2 种方案生成 Change log 的工具可供选择

1 [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog)

2 进入 [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) 链接发现官方提供了更新的发布工具 更好的选择是 [standard-version](https://github.com/conventional-changelog/standard-version)

### standard-version

使用 conventional-changelog 会自动添加新的本地 git tag 和 改变 npm version 更多说明请到链接处查找

```bash
npm i --save-dev standard-version

# Add an npm run script to your package.json:
{
  "scripts": {
    "release": "standard-version"
  }
}

#Install globally (add to your PATH):、
npm i -g standard-version

#  First Release
# npm run script
npm run release -- --first-release
# or global bin
standard-version --first-release

# new release
# npm run script
npm run release
# or global bin
standard-version

Simply add the following to your package.json to configure lifecycle scripts:
{
  "standard-version": {
    "scripts": {
      "prebump": "echo 9.9.9"
    }
  }
}
```

### conventional-changelog

conventional-changelog 会生成一个 CHANGELOG.md 文件

```bash
$ npm install -g conventional-changelog-cli
$ cd my-project
$ conventional-changelog -p angular -i CHANGELOG.md -w
```

上面命令不会覆盖以前的 Change log，只会在 CHANGELOG.md 的头部加上自从上次发布以来的变动。如果你想生成所有发布的 Change log，要改为运行下面的命令。

```bash
$ conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md
```

为了方便使用，可以将其写入 package.json 的 scripts 字段。

```bash
{
  "scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -r 0 && git add CHANGELOG.md"
  }
}
```

以后，直接运行下面的命令即可。

```bash
$ npm run changelog
```

### 参考链接

> [git 提交信息指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
