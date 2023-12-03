# @actions/github 发布版本

### 6.0.0 
- 支持最新的 Octokit 在 @actions/github 中 [#1553](https://github.com/actions/toolkit/pull/1553)
  - 放弃对 NodeJS v14、v16 的支持

### 5.1.1
- 导出默认的 octokit 选项 [#1188](https://github.com/actions/toolkit/pull/1188)

### 5.1.0
- 向 getOctokit 方法添加 additionalPlugins 参数 [#1181](https://github.com/actions/toolkit/pull/1181)
- 依赖更新 [#1180](https://github.com/actions/toolkit/pull/1180)

### 5.0.3
- 更新到 `@actions/http-client` 的 v2.0.1 [#1087](https://github.com/actions/toolkit/pull/1087)

### 5.0.2
- 更新到 `@actions/http-client` 的 v2.0.0

### 5.0.1
- [更新 Octokit 依赖](https://github.com/actions/toolkit/pull/1037)
### 5.0.0
- [更新 @actions/github 以包括最新的 octokit 定义](https://github.com/actions/toolkit/pull/783)
- [在上下文中添加 urls](https://github.com/actions/toolkit/pull/794)

### 4.0.0
- [向上下文添加执行状态信息](https://github.com/actions/toolkit/pull/499)
- [使用一些 API 破坏性更改更新 Octokit 依赖项](https://github.com/actions/toolkit/pull/498) 
  - 完整的 API 更改列表在[此处](https://github.com/octokit/plugin-rest-endpoint-methods.js/releases/tag/v4.0.0)
  - `GitHub.plugin()` 不再支持数组作为第一个参数。必须传入多个参数。

### 3.0.0
- [切换到 @octokit/core 并使用插件利用最新的 Octokit APIs](https://github.com/actions/toolkit/pull/453)
- [在负载上下文中添加评论字段](https://github.com/actions/toolkit/pull/375) 

### 2.2.0

- [支持 GHES：使用 GITHUB_API_URL 和 GITHUB_GRAPHQL_URL 确定 baseUrl](https://github.com/actions/toolkit/pull/449)

### 2.1.1

- [使用 import {Octokit}](https://github.com/actions/toolkit/pull/332)
- [在设置代理代理之前检查代理绕过](https://github.com/actions/toolkit/pull/320)

### 2.1.0

- [Octokit 客户端遵循代理设置](https://github.com/actions/toolkit/pull/314)
- [修复拉取请求评论事件的问题编号](https://github.com/actions/toolkit/pull/311)

### 2.0.1

- 在 package.json 中添加 "types" [#221](https://github.com/actions/toolkit/pull/221)

### 2.0.0

- 将 Octokit 版本升级到 4.x 以包含 TypeScript 类型 [#228](https://github.com/actions/toolkit/pull/228)

### 1.1.0

- 在 GitHub 构造函数中接受 Octokit.Options [#113](https://github.com/actions/toolkit/pull/113)

### 1.0.1

- 通过删除动态 require 简化 WebPack 配置 - [#101](https://github.com/actions/toolkit/pull/101)

### 1.0.0

- 初始发布