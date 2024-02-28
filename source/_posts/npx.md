---
title: 理解npx - npm package runner
date: 2022-10-20 00:39:04
updated: 2022-10-20 00:39:04
categories:
    - 入门
tags:
    - Node.js
---

[How to use npx: the npm package runner](https://blog.scottlogic.com/2018/04/05/npx-the-npm-package-runner.html)
[npx | npm Docs](https://docs.npmjs.com/cli/v7/commands/npx)

## 应用场景

- 避免安装global package
- 调用项目内部安装的模块




## 以React Native为例子

React Native 新建项目：

```shell

```

[react-native/package.json](https://www.runpkg.com/?react-native@0.70.3/package.json)

```json
{
    "name": "react-native",
    "version": "0.70.3",
    "bin": "./cli.js"
}
```

[react-native/cli.js](https://www.runpkg.com/?react-native@0.70.3/cli.js)

```javascript
'use strict';

var cli = require('@react-native-community/cli');

if (require.main === module) {
  cli.run();
}

module.exports = cli;
```

[react-native-community/cli/build/index.js](https://www.runpkg.com/?@react-native-community/cli@9.1.3/build/index.js)



```javascript
// ...
const program = new (_commander().Command)().usage('[command] [options]').version(pkgJson.version, '-v', 'Output the current version').option('--verbose', 'Increase logging verbosity');
// ...
async function run() {
  try {
    await setupAndRun();
  } catch (e) {
    handleError(e);
  }
}

async function setupAndRun() {
  // Commander is not available yet
  // when we run `config`, we don't want to output anything to the console. We
  // expect it to return valid JSON
  if (process.argv.includes('config')) {
    _cliTools().logger.disable();
  }

  // ...
  program.parse(process.argv); // 从process.argv中拿到命令行参数
}

```

