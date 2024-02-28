---
title: 读Lodash源码 - difference
date: 2022-11-30 11:48:19
updated: 2022-11-30 11:48:19
categories:
tags:
- 读源码
---

## 使用

```js
_.difference(array, [values])
```

该方法用于对数组进行过滤，排除给定的值，剩下的值组成数组返回。

- `array` _(Array)_: 目标数组
- `[values]` _(...Array)_: 要排除的值组成的数组

```js
_.difference([1, 2, 2, 3, 3], [1, 2, 2]) //[ 3, 3 ]
```

## 代码

核心逻辑在[`baseDifference.js`](https://github.com/lodash/lodash/blob/master/difference.js)中

```js
import SetCache from './SetCache.js'
import arrayIncludes from './arrayIncludes.js'
import arrayIncludesWith from './arrayIncludesWith.js'
import map from '../map.js'
import cacheHas from './cacheHas.js'

/** Used as the size to enable large array optimizations. */
const LARGE_ARRAY_SIZE = 200

/**
 * The base implementation of methods like `difference` without support
 * for excluding multiple arrays.
 *
 * @private
 * @param {Array} array The array to inspect.
 * @param {Array} values The values to exclude.
 * @param {Function} [iteratee] The iteratee invoked per element.
 * @param {Function} [comparator] The comparator invoked per element.
 * @returns {Array} Returns the new array of filtered values.
 */
function baseDifference(array, values, iteratee, comparator) {
  let includes = arrayIncludes
  let isCommon = true
  const result = []
  const valuesLength = values.length

  if (!array.length) {
    return result
  }
  if (iteratee) {
    values = map(values, (value) => iteratee(value))
  }
  if (comparator) {
    includes = arrayIncludesWith
    isCommon = false
  }
  else if (values.length >= LARGE_ARRAY_SIZE) {
    includes = cacheHas
    isCommon = false
    values = new SetCache(values)
  }
  outer:
  for (let value of array) {
    const computed = iteratee == null ? value : iteratee(value)

    value = (comparator || value !== 0) ? value : 0
    if (isCommon && computed === computed) {
      let valuesIndex = valuesLength
      while (valuesIndex--) {
        if (values[valuesIndex] === computed) {
          continue outer
        }
      }
      result.push(value)
    }
    else if (!includes(values, computed, comparator)) {
      result.push(value)
    }
  }
  return result
}

export default baseDifference
```

## 参考

[lodash/baseDifference.js at master · lodash/lodash · GitHub](https://github.com/lodash/lodash/blob/master/.internal/baseDifference.js)

[Lodash 文档](https://lodash.com/docs/#difference)

