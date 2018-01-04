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
* style： 格式（不影响代码运行的变动）
* refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
* test：增加测试
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

[git 提交信息指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
