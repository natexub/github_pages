---
title: 给命令行设置代理
date: 2020-09-25 19:08:08
updated: 2020-09-25 19:08:08
categories:
tags:
---

本文描述了如何给命令行设置代理

## 概述

约定俗成，通过指定环境变量`https_proxy`和`http_proxy`的方法，告诉应用代理情况。下文只用`https_proxy`为例。

## 变量值格式

`https_proxy`使用 URL 格式的代理服务器的主机名或 IP 地址设置，如下例所示：

```
https_proxy=http://proxy.example.com
```

如果代理服务器需要用户名和密码，您必须包含凭据，如下例所示：

```
https_proxy=http://username:password@proxy.example.com
```

如果代理服务器使用 80 以外的端口，则必须包含端口号，如下例所示：

```
https_proxy=http://username:password@proxy.example.com:8080
```

如果代理服务器是 SOCKS5 代理，则必须在 URL 中指定 SOCKS5 协议，如下例所示：

```
https_proxy=socks5://socks_proxy.example.com
```

## 在 Mac OS 或 Linux 中设置 https_proxy

`https_proxy`在 Mac OS 或 Linux 中设置环境变量：

1.  使用特定于您的 shell 的命令。例如，在 bash 中，使用`export`命令，如下例所示：
    
    ```
    export https_proxy=http://my.proxyserver.com:8080
    ```
    
2.  要使此更改持久化，请将命令添加到 shell 的相应配置文件中。例如，在 bash 中，在您的`.bash_profile`or`.bashrc`文件中添加如下示例所示的行：
    
    ```
    https_proxy=http://username:password@hostname:port
    export $https_proxy
    ```

## 在 Windows 中设置 https_proxy

环境变量设置

| 键           |                        值 |
|:------------|-------------------------:|
| http_proxy  | http://127.0.0.1:10809 |
| https_proxy | http://127.0.0.1:10809 |


## 扩展

一般还要配合`NO_PROXY`变量来避免回环等情况。对大小写有疑问见参考2。

| 键        |                   值 |
|:---------|--------------------:|
| NO_PROXY | localhost,127.0.0.1,::1 |

注：_::1_ is the loopback address in IPv6. Think of it as the IPv6 version of 127.0.0.1

## 参考

1. [Using the cf CLI with a Proxy Server | Cloud Foundry Docs](https://docs.cloudfoundry.org/cf-cli/http-proxy.html)
2. [http - Are HTTP_PROXY, HTTPS_PROXY and NO_PROXY environment variables standard? - Super User](https://superuser.com/questions/944958/are-http-proxy-https-proxy-and-no-proxy-environment-variables-standard)（已缓存）
