# `@actions/http-client`

一个专为构建 GitHub Actions 优化的轻量级 HTTP 客户端。

## 特性

  - 带有 TypeScript 泛型和 async/await/Promises 的 HTTP 客户端
  - 包含类型定义！
  - [代理支持](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-self-hosted-runners#using-a-proxy-server-with-self-hosted-runners) 在 GitHub Actions 和 Runner 中自动生效
  - 面向 ES2019（runner 在 node 12+ 上运行 actions）。仅支持 node 12+。
  - 基本、Bearer 和 PAT 支持开箱即用。支持其他处理程序的扩展。
  - 支持重定向

查看[此处](./RELEASES.md)的特性和版本发布。

## 安装

```
npm install @actions/http-client --save
```

## 示例

详细示例请参阅[测试](./__tests__)。

## 错误

### HTTP

HTTP 客户端只有在真正异常时才会抛出错误。

 * 成功执行的请求，返回 404、500 等状态码时，将返回一个带有状态码和正文的响应对象。
 * 默认情况下会跟随重定向（3xx）。

详细示例请参阅[测试](./__tests__)。

## 调试

要启用所有 HTTP 请求和响应的详细控制台日志记录，请设置 NODE_DEBUG 环境变量：

```shell
export NODE_DEBUG=http
```

## Node 支持

`http-client` 使用 Node 12 的最新 LTS 版本构建。它可能在之前的 node LTS 版本上工作，但在 Node12+ 上经过测试并得到官方支持。

## 支持和版本控制

我们遵循 semver，并将在主版本之间保持兼容性，并通过增加次版本来引入新的功能和功能（同时保持兼容性）。

## 贡献

我们欢迎 PR。请在进行代码之前创建一个 issue，并在适用的情况下，提供设计。

首次运行：

```
npm install
```

构建：

```
npm run build
```

运行所有测试：

```
npm test
```