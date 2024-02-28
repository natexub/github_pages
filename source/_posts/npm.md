---
title: npm
date: 2019-11-08 04:13:38
updated: 2019-11-08 04:13:38
categories:
tags:
---

## NPM 配置

### npm配置文件

npm通过三种方式获取配置：
- 命令行
- 环境变量
- `npmrc`文件

`npm config`命令会修改全局`npmrc`文件。`npmrc`文件有多个：
-   项目下 (`/path/to/my/project/.npmrc`)
-   用户目录(`~/.npmrc`或者windows下的`%USERPROFILE%\.npmrc`)
-   global config file ($PREFIX/etc/npmrc)
-   npm安装位置下 (/path/to/npm/npmrc)，不可更改，主要是为了分发标准覆盖默认配置。
这些文件中的每一个都会被加载，按优先级顺序解析，从上到下优先级递减，优先级高的配置会覆盖优先级低的配置。

### npm可配置项

全部可配置项见 [config | npm Docs](https://docs.npmjs.com/cli/v8/using-npm/config/)。
通常需要理解的配置项目有：

#### `prefix` - 全局项目的安装位置。
- 默认值: 全局模式时，node可执行程序的位置。In local mode, the nearest parent folder containing either a package.json file or a node_modules folder.
- Type: Path

上文中给Node.js配置了环境变量，把node的执行文件目录追加进了`Path`。默认`prefix`设置成和它在一起，使得这些全局项目的可执行文件同时可被找到。如果遇到某些成功全局安装的包却在命令行中不可用，就要考虑这里的问题。

#### `cache` - npm cache目录路径
- 默认值: Windows: `%LocalAppData%\npm-cache`, Posix: `~/.npm`
- Type: Path

#### `registry` - npm仓库的base URL
- 默认值：`"https://registry.npmjs.org/"`
-  Type: URL


## NPM脚本

[npm-run-script | npm Docs](https://docs.npmjs.com/cli/v9/commands/npm-run-script)

默认根据平台选择运行脚本的Shell

除了预先存在的`PATH`变量，`npm run`还会向`PATH`变量追加项目路径下的`node_modules/.bin`。这样项目安装的依赖下的可执行文件可以在没有`node_modules/.bin`前缀的情况下使用。例如：
```json
"scripts": {"test": "tap test/*.js"}
```
可以代替
```json
"scripts": {"test": "node_modules/.bin/tap test/*.js"}
```

## NPM 路径问题

见[folders | npm Docs](https://docs.npmjs.com/cli/v9/configuring-npm/folders#executables)

## NPM 报错

### npm audit报错

``` powershell
PS C:\Users\xuyan> npm install -g nrm
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142

changed 73 packages, and audited 317 packages in 5s

6 vulnerabilities (4 high, 2 critical)

To address all issues, run:
  npm audit fix

Run `npm audit` for details.
```

npm 审计报错，不用管。