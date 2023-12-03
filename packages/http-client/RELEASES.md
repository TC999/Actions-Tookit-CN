## 发布版本

## 2.2.0
- 添加返回代理代理调度程序的功能，以与最新的 octokit 包兼容 [#1547](https://github.com/actions/toolkit/pull/1547)

## 2.1.1
- 添加 `HttpClientResponse.readBodyBuffer` 方法，从响应流中读取并返回缓冲区 [#1475](https://github.com/actions/toolkit/pull/1475)

## 2.1.0
- 添加对 `no_proxy` 中的 `*` 和子域的支持 [#1355](https://github.com/actions/toolkit/pull/1355) [#1223](https://github.com/actions/toolkit/pull/1223)
- 为回环 IP 地址绕过代理添加支持 [#1360](https://github.com/actions/toolkit/pull/1360))

## 2.0.1
- 修复缺少 `tunnel` 依赖项的问题 [#1085](https://github.com/actions/toolkit/pull/1085)

## 2.0.0
- 该包现在使用 TypeScript 的 [`strict` 编译器设置](https://www.typescriptlang.org/tsconfig#strict) 进行编译。为了符合更严格的规则：
  - 一些导出的类型现在包括 `| null` 或 `| undefined`，与它们的实际行为相匹配。
  - 实现了方法 `RequestHandler.handleAuthentication()` 的类型现在在不支持处理 HTTP 401 响应时抛出 `Error` 而不是返回 `null`。调用方仍然可以使用 `canHandleAuthentication()` 来确定是否支持此处理。
  - 使用 `any` 的类型已限定为更具体的类型。
- 遵循 TypeScript 的命名约定，导出的接口不再以前缀 `I-` 开头。
- 删除了 `IHttpClientResponse` 接口，采用 `HttpClientResponse` 类。
- 删除了 `IHeaders` 接口，采用 `http.OutgoingHttpHeaders`。
- 该包的源代码已移至与 [actions/toolkit](https://github.com/actions/toolkit) 一起构建。

## 1.0.11

包含一个修复代理未定义用户和密码的 bug。查看 [此处的 PR](https://github.com/actions/http-client/pull/42)。

## 1.0.9
在服务器响应非成功状态码时，从 \<verb>Json() 辅助方法中抛出 `HttpClientError` 而不是通用的 `Error`。

## 1.0.8
修复了一个安全问题，即重定向（例如 302）到另一个域时会传递头部。修复方法是如果主机名不同，则剥离授权头。更多 [PR #27 中的详细信息](https://github.com/actions/http-client/pull/27)

## 1.0.7
更新 NPM 依赖项并将 429 添加到 HttpCodes 列表

## 1.0.6
如果在客户端或参数中未设置，则为 \<verb>Json() 辅助方法自动发送 Content-Type 和 Accept application/json 头。

## 1.0.5
为 json over http 场景添加了 \<verb>Json() 辅助方法。

## 1.0.4
开始添加 \<verb>Json() 辅助方法。请勿使用此版本。使用 >= 1.0.5，因为类型存在问题。

## 1.0.1 到 1.0.3
添加代理支持。