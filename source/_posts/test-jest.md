---
title: test-jest
date: 2022-03-25 21:59:42
updated: 2022-03-25 21:59:42
categories:
tags:
- Test
- Jest
---

## toBe和toEqual的不同

[Expect · Jest](https://jestjs.io/docs/expect#toequalvalue)


## config

[Configuring Jest · Jest](https://jestjs.io/docs/configuration)

### 1. 配置文件

自动发现配置文件 `jest.config.js|ts|mjs|cjs|json`

### 2. package.json

字段:

```json
{
	"jest": {  
		"verbose": true  
	}
}
```

## 向测试代码提供dom环境

[Configuring Jest · Jest](https://jestjs.io/docs/configuration#testenvironment-string)

```
yarn add --dev jest-environment-jsdom
```

```
npm install --save-dev jest-environment-jsdom
```

配置：
```
testEnvironment: 'jsdom',
```