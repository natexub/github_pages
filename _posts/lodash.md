---
title: Lodash 使用
date: 2019-11-22 18:56:32
updated: 2019-11-22 18:56:32
categories:
tags:
---

[中文文档](https://www.lodashjs.com/)|[英文文档](https://lodash.com/docs/)

### 1. 依赖和引入

#### 1.1 Ant Design

通过查看Ant Design Vue 的依赖(`package.json`)发现，Antdv已经对Lodash进行了依赖。而且antd默认通过[babel-plugin-import](https://github.com/ant-design/babel-plugin-import#readme) 插件来实现[按需加载](https://ant.design/docs/react/getting-started-cn#%E6%8C%89%E9%9C%80%E5%8A%A0%E8%BD%BD)。所以可以跳过调整依赖步骤，直接使用即可(方法一)。

##### 1.1.1 方法一

既然Antd已经添加了对Lodash的依赖,直接对node_modules中的lodash引用使用即可:

```javascript
import add from 'lodash/add'
import { isEqual } from 'lodash'
import _ from 'lodash'

console.log(add(1, 2), isEqual(result, 3)) //3 true
console.log(_.times(3, String))// => ['0', '1', '2']
```

- 缺点: IDE没有在当前模块的package.json所写明的依赖中找到`lodash/add`所以会有不规范提示。要消除提示在当前项目的`package.json`中再添加对lodash 的依赖即可。

##### 1.1.2 方法二

先在`package.json`中添加依赖

```javascript
"dependencies": {
    // ...
    "lodash.add": "^4.5.0",
    // ...
}
```

```javascript
import add from 'lodash.add'

let result = add(1,2)
console.log(result) //3
```
Ant design pro的源码中就采用方法二对偶尔使用的lodash方法进行引用使用。

- 缺点 相关库文件作为独立于lodash的模块引用可能徒增打包大小, 比如`lodash.add`和`lodash/add`会被分贝打包到两个Trunk中。官方在[Per Method Packages](https://lodash.com/per-method-packages)中写明不鼓励使用这些模块包。(当然根据所开发web应用的使用场景，如果一两个函数几k的大小不会影响很大，没必要花精力过早优化)

#### 1.2 Antdv外的情况

Lodash完整库体积不小，但是使用时只会涉及到其中的小部分，引入使用时就需要考虑需不需要按需引用。同时大量引用使用lodash时还要考虑引用造成的维护问题。Antd默认通过[babel-plugin-import](https://github.com/ant-design/babel-plugin-import#readme)来实现了[按需加载](https://ant.design/docs/react/getting-started-cn#%E6%8C%89%E9%9C%80%E5%8A%A0%E8%BD%BD)，所以不需要考虑这种情况。如果按照官方文档的说法，还是要探究一下引用的方式。

按需引用不是说考虑添加依赖的是不是太多，而是说考虑打包后的体积是不是够轻，因为添加的依赖只有部分会被Webpack拿去打包。要分析打包大小vue-cli可以使用脚本`"vue-cli-service build --report`, 这是[vue-cli](https://cli.vuejs.org/zh/guide/cli-service.html#vue-cli-service-build)对[webpack-bundle-analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)的封装

##### 1.2.1 在不用额外插件的情况下可以有三种方式来引用

1. 引入整个Lib

   ```javascript
   import _ from 'lodash';
   
   _.add(1,2)
   ```

   - **优点**: 只需要写一行引用

   - **缺点**:  会导致整个库都被打包，而且具体引用变得不清晰使代码可读性变差,还会。

2. 通过大括号来引入特定的方法

   ```javascript
   import { add, tail, times, uniq } from 'lodash'
   
   add(1,2)
   ```
   - **优点**: 只需要写一行引用，可读性强一点使用`add()`而不是`_.add()`
   
   - **缺点**: 仍然会导致整个库都被打包，并且当我们代码引用变动时还需要去维护这条引用语句
   
3. 每个方法通过每个函数模块单独引入([Per Method Packages](https://lodash.com/per-method-packages))

   ```javascript
   import add from 'lodash/map';
   import tail from 'lodash/tail';
   import times from 'lodash/times';
   import uniq from 'lodash/uniq';
   ```

   另外有一种和这个方法相同效果的一种方法。Lodash中的函数在NPM仓库中都有一个单独的发布模块([NPM: results for ‘lodash'](https://www.npmjs.com/search?q=lodash))，当然前提是在package.json中写明引入的具体函数模块的依赖。同样的写法就成了:

   ```javascript
   import add from 'lodash.map';
   import tail from 'lodash.tail';
   import times from 'lodash.times';
   import uniq from 'lodash.uniq';
   ```

   - **优点**: 可以得到最小的打包，可使用`add()`而不是`_.add()`，可读性强一点。

   - **缺点**: 当我们代码引用变动时, 要维护在文件中的import，用函数模块方式的话还要维护在`package.json`中的模块依赖，很麻烦。同时一个文件中太多行的引用让可读性变差。

**在简单场景下，方式3的两个方法足以按需加载的问题。**但是我们不得不在一个文件中引入使用大量Lodash方时，带来的繁琐的维护就需要考虑其他的方法。

##### 1.2.2 使用插件: 

通过使用插件: `Lodash-es` ，`Lodash`是采用的往全局对象(window)上挂载一个复杂对象的方法，而`Lodash-es`就是将单个方法export出来的方法。所以说`Lodash`不支持tree-shaking，我们按需加载需要使用`Lodash-es`。也就是官方文档[Module Formats](https://www.lodashjs.com/#%E6%A8%A1%E5%9D%97%E6%A0%BC%E5%BC%8F)中说的[lodash-es](https://www.npmjs.com/package/lodash-es) + [babel-plugin-lodash](https://www.npmjs.com/package/babel-plugin-lodash) + [lodash-webpack-plugin](https://www.npmjs.com/package/lodash-webpack-plugin)组合的方式。

完整的Lodash-es的bundle大小有256K，babel插件`babel-plugin-lodash`的原理就是根据全量导入的变量的调用链, 分析所需要的模块, 然后按需导入.

而lodash-webpack-plugin是做进一步缩小。

> Create smaller Lodash builds by replacing [feature sets](https://www.npmjs.com/package/lodash-webpack-plugin#feature-sets) of modules with [noop](https://lodash.com/docs#noop), [identity](https://lodash.com/docs#identity), or simpler alternatives.
>
> This plugin complements [babel-plugin-lodash](https://www.npmjs.com/package/babel-plugin-lodash) by shrinking its cherry-picked builds even further!

**配置方法(Vue-CLI):**

安装依赖

```bash
yarn add lodash-webpack-plugin  babel-plugin-lodash  --dev
```

在vue.config.js中配置

```javascript
//按需加载lodash
const LodashModuleReplacementPlugin = require("lodash-webpack-plugin");

module.exports = {
  chainWebpack: config => {
    //按需加载lodash
    if (process.env.NODE_ENV === "production") {
      config.plugin("loadshReplace").use(new LodashModuleReplacementPlugin());
    };
  }
}
```

在babel.config.js中配置

```javascript
module.exports = {
  "plugins":["lodash"]
}
```



那同样是通过Babel插件转换实现的按需导入，阿里的[babel-plugin-import](https://github.com/ant-design/babel-plugin-import#readme)和[babel-plugin-lodash](https://www.npmjs.com/package/babel-plugin-lodash) 原理有什么不同呢？

`babel-plugin-import`是阿里为了`antd`组件库量身定做的一套转换工作, 不过在后续的更新中, 适用的范围越来越广. 它除了会导入目标组件外, 还支持导入组件附属样式文件. 

```javascript
import { Button } from 'antd';

ReactDOM.render(<Button>xxxx</Button>);
```

转换成:

```javascript
var _button = require('antd/lib/button');

require('antd/lib/button/style/css');

ReactDOM.render(<_button>xxxx</_button>);
```

`babel-plugin-lodash`插件会将

```javascript
import _ from 'lodash'
import { add } from 'lodash/fp'

const addOne = add(1)
_.map([1, 2, 3], addOne)
```

转换成

```javascript
import _add from 'lodash/fp/add'
import _map from 'lodash/map'

const addOne = _add(1)
_map([1, 2, 3], addOne)
```



使用插件的**缺点**: 当涉及的模块繁多时，全量构建时间就会大大延长，而同时对打包进行 gzip压缩使打包的体积差异可以忽略不计。这时候就要考虑使用插件是否值得。

### 模块组成

- Array 数组
- Collocation 集合
- Date 日期
- Function函数
- Lang 语言
- Math 数学
- Number 数字
- Object 对象
- Seq 链式调用
- String 字符串
- Util 使用函数
- Properties 和 Method (不常用)

### 常用方法



### [目录](https://xiaoxiami.gitbook.io/lodash/summary)

