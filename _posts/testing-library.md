---
title: testing-library
date: 2022-01-30 14:00:18
updated: 2022-01-30 14:00:18
categories:
tags:
- React
- 测试
---

## 涉及到的项目

### jest-dom

通过提供一组 custorm jest Matcher 扩展了 jest。

[@testing-library/jest-dom](https://github.com/testing-library/jest-dom)

[jest-dom | Testing Library](https://testing-library.com/docs/ecosystem-jest-dom)

### React Testing Library

[@testing-library/react](https://github.com/testing-library/react-testing-library)

[React Testing Library | Testing Library](https://testing-library.com/docs/react-testing-library/intro)

### user-event

提供用户事件交互模拟。

[@testing-library/user-event](https://github.com/testing-library/user-event)

[Introduction | Testing Library](https://testing-library.com/docs/user-event/intro/)

## 用户交互

通过事件模拟用户交互。

### `fireEvent`

### `user-event`

[V14.0.0](https://github.com/testing-library/user-event/releases/tag/v14.0.0)引入很多 breaking changes 。例如，引入`userEvent.setup`API

```js
import userEvent from '@testing-library/user-event'  

const user = userEvent.setup()
await user.click(screen.getByText('Check')) // 点击显示Check的元素
```

为什么要在模拟事件之前进行 setup 呢？
[Setup | Testing Library](https://testing-library.com/docs/user-event/setup/)

### `user-event`和`fireEvent`比较

fireEvent调度DOM 事件，而user-event模拟完整 的交互，这可能会触发多个事件并在此过程中进行额外的检查。


### Mock

Jest 的 Mock 函数用于测试组件是否会调用其绑定的回调以响应特定事件。

```js
import {render, screen, fireEvent} from '@testing-library/react'  
  
const Button = ({onClick, children}) => (  
<button onClick={onClick}>{children}</button>  
)  
  
test('calls onClick prop when clicked', () => {  
const handleClick = jest.fn()  
render(<Button onClick={handleClick}>Click Me</Button>)  
fireEvent.click(screen.getByText(/click me/i))  
expect(handleClick).toHaveBeenCalledTimes(1)
```

## class names 测试

```js
// ...
const element = screen.getByText('Danger')
expect(element).toHaveClass(`btn btn-danger btn-lg klass`)
```

class属性中子字符串的顺序可能不确定。比如：

```
Error: expect(element).toHaveClass("btn btn-danger btn-lg klass")

Expected the element to have class:
  btn btn-danger btn-lg klass
Received:
  btn Klass btn-danger btn-lg
```

[`.toHaveClass`](https://github.com/testing-library/jest-dom#tohaveclass) 由`@testing-library/jest-dom`提供。
toHaveClass的类型描述：

```ts
// matchers.d.ts
toHaveClass(...classNames: string[]): R;  
toHaveClass(classNames: string, options?: { exact: boolean }): R;
```

把测试语句改成这种即可：

```js
expect(element).toHaveClass('btn','btn-danger','btn-lg', 'btn-lg')
```

### 测试Hook
[API Reference | React Hooks Testing Library](https://react-hooks-testing-library.com/reference/api)
#### 如何测试返回jsx的函数？