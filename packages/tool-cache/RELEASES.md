# @actions/tool-cache Releases

### 2.0.1
- 将 `@actions/http-client` 更新到 v2.0.1 [#1087](https://github.com/actions/toolkit/pull/1087)

### 2.0.0
- 将 `@actions/http-client` 更新到 v2.0.0
- 导出函数 `downloadTool` 中 `headers` 参数的类型已经从 `{ [header: string]: any }` 缩小到 `{ [header: string]: number | string | string[] | undefined; }`（即 `http.OutgoingHttpHeaders`）。
  这仅是 TypeScript 消费者的编译时更改。以前尝试使用不在新接受的类型之一的标头值将导致运行时错误。

### 1.7.2
- 将 `lockfileVersion` 更新到 `v2` 在 `package-lock.json` 中 [#1025](https://github.com/actions/toolkit/pull/1025) 

### 1.7.1
- [在获取 Linux 版本时备用使用 os-releases 文件](https://github.com/actions/toolkit/pull/594)
- [更新到最新的 @actions/io 版本](https://github.com/actions/toolkit/pull/838)

### 1.7.0
- [在下载工具到 tc 时允许任意标头](https://github.com/actions/toolkit/pull/530)
- [导出 `isExplicitVersion` 和 `evaluateVersions` 函数](https://github.com/actions/toolkit/pull/796) 
- [默认在提取已压缩文件时强制覆盖](https://github.com/actions/toolkit/pull/807)

### 1.6.1
- [更新 @actions/core 版本](https://github.com/actions/toolkit/pull/636)

### 1.6.0
- [添加 `extractXar` 函数来提取 XAR 文件](https://github.com/actions/toolkit/pull/207)

### 1.3.5

- [在执行之前检查工具路径是否存在](https://github.com/actions/toolkit/pull/385)
- [使提取函数默认保持安静](https://github.com/actions/toolkit/pull/206)

### 1.3.4

- [将 http-client 更新到 1.0.8，其中包含安全修复](https://github.com/actions/toolkit/pull/429)

这是 [在 http-client 1.0.8 版本中修复的安全问题](https://github.com/actions/http-client/pull/27)

### 1.3.3

- [将 downloadTool 更新为仅重试 500、408 和 429](https://github.com/actions/toolkit/pull/373)

### 1.3.2

- [通过更好的错误处理和重试更新 downloadTool](https://github.com/actions/toolkit/pull/369)

### 1.3.1

- [增加 http-client 的最小版本要求](https://github.com/actions/toolkit/pull/314)

### 1.3.0

- [使用 @actions/http-client](https://github.com/actions/http-client)

### 1.2.0

- [重载 downloadTool 以接受目标路径](https://github.com/actions/toolkit/pull/257)
- [在 Windows 上修复 `extractTar`](https://github.com/actions/toolkit/pull/264)
- [添加 "types" 到 package.json](https://github.com/actions/toolkit/pull/221)

### 1.1.2

- [从 PATH 中使用 zip 和 unzip](https://github.com/actions/toolkit/pull/161)
- [为 `extractTar` 支持自定义标志](https://github.com/actions/toolkit/pull/48)

### 1.0.0

- 初始发布