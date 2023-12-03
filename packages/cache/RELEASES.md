# @actions/cache 发布版本

### 0.1.0

- 初始发布

### 0.2.0

- 修复了在 Windows 和 Ubuntu 16.04 上使用 zstd 压缩算法的问题 [#469](https://github.com/actions/toolkit/pull/469)

### 0.2.1

- 修复等待异步函数 `getCompressionMethod` 的问题

### 1.0.0

- 使用 Azure SDK 提高速度和可靠性下载 Azure 托管的缓存
- 显示下载进度
- 包括一些破坏与早期版本兼容性的更改，包括：
  - `retry`、`retryTypedResponse` 和 `retryHttpClientResponse` 从 `cacheHttpClient` 移动到 `requestUtils`

### 1.0.1

- 修复使用 Azure SDK 下载大文件（> 2 GB）的错误

### 1.0.2

- 使用 posix 存档格式以支持某些工具

### 1.0.3

- 使用 http-client v1.0.9
- 修复错误处理，以便在非可重试错误（例如 409 冲突）上不进行重试
- 在重试尝试之间添加 5 秒延迟

### 1.0.4

- 使用 @actions/core v1.2.6
- 修复 `uploadChunk`，如果收到任何不成功的响应代码，则抛出错误

### 1.0.5

- 修复确保 Windows 缓存路径正确解析的问题

### 1.0.6

- 使缓存更加详细说明 [#650](https://github.com/actions/toolkit/pull/650)
- 在 macOS 上如果可用，则使用 GNU tar [#701](https://github.com/actions/toolkit/pull/701)

### 1.0.7

- 修复在 macOS 上使用 GNU tar 提取存档时的权限问题（[问题](https://github.com/actions/cache/issues/527))

### 1.0.8

- 将允许的工件缓存大小从 5GB 增加到 10GB（[问题](https://github.com/actions/cache/discussions/497))

### 1.0.9

- 使用 @azure/ms-rest-js v2.6.0
- 使用 @azure/storage-blob v12.8.0

### 1.0.10

- 在 `package-lock.json` 中将 `lockfileVersion` 更新为 `v2` [#1022](https://github.com/actions/toolkit/pull/1022)

### 1.0.11

- 修复文件下载 > 2GB 的问题（[问题](https://github.com/actions/cache/issues/773))

### 2.0.0

- 增加对检查 Actions 缓存服务功能是否可用的支持 [#1028](https://github.com/actions/toolkit/pull/1028)

### 2.0.3

- 更新到 `@actions/http-client` 的 v2.0.0

### 2.0.4

- 更新到 `@actions/http-client` 的 v2.0.1 [#1087](https://github.com/actions/toolkit/pull/1087)

### 2.0.5

- 修复当没有文件可用于缓存时避免保存空缓存的问题（[问题](https://github.com/actions/cache/issues/624))

### 2.0.6

- 修复在创建 tar 的临时目录实际上是用户用于缓存的路径的子目录时出现的 `Tar 失败，错误为：进程 '/usr/bin/tar' 以退出码 1 失败` 的问题（[问题](https://github.com/actions/cache/issues/689))

### 3.0.0

- 更新 actions/cache 以抑制 Actions 缓存服务器错误，并对这些错误记录警告 [#1122](https://github.com/actions/toolkit/pull/1122)

### 3.0.1

- 修复 [#833](https://github.com/actions/cache/issues/833) - 缓存在 GitHub 工作区目录上不起作用的问题。
- 修复 [#809](https://github.com/actions/cache/issues/809) 在 AWS 自托管运行器上出现 `zstd -d: 文件或目录不存在` 错误的问题。

### 3.0.2

- 为下载卡住的问题添加了 1 小时超时 [#810](https://github.com/actions/cache/issues/810).

### 3.0.3

- 修复下载卡住的问题的 Bug 修复 [#810](https://github.com/actions/cache/issues/810).

### 3.0.4

- 修复在 gnu tar 中 zstd 不起作用的问题，涉及到问题 [#888](https://github.com/actions/cache/issues/888) 和 [#891](https://github.com/actions/cache/issues/891).
- 允许用户通过环境变量 `SEGMENT_DOWNLOAD_TIMEOUT_MINS` 提供自定义超时，以中止缓存段的下载。默认为 60 分钟。

### 3.0.5

- 更新 `@actions/cache` 以使用 `@actions/core@^1.10.0`

### 3.0.6

- 在依赖项中添加了 `@azure/abort-controller` 以解决与 ESM 的兼容性问题 [#1208](https://github.com/actions/toolkit/issues/1208)

### 3.1.0-beta.1

- 更新 Windows 上的 actions/cache，以默认使用 gnu tar 和 zstd，并在 gnu tar 不可用时回退到 bsdtar 和 zstd。([问题](https://github.com/actions/cache/issues/984))

### 3.1.0-beta.2

- 添加对在 Windows 上还原旧缓存时回退到 gzip 的支持。

### 3.1.0-beta.3

- 修复在 Windows 上回退到 gzip 以还原旧缓存时和如果 gnutar 不可用时回退到 bsdtar 的 Bug 修复。

### 3.1.0

- 更新 Windows 上的 actions/cache，以默认使用 gnu tar 和 zstd
- 更新 Windows 上的 actions/cache，以在 gnu tar 不可用时回退到 bsdtar 和 zstd。
- 添加对在 Windows 上还原旧缓存时回退到 gzip 的支持。

### 3.1.1

- 撤销 

3.1.0 中的更改，以修复 Windows 上符号链接还原的问题。
- 添加对缓存未命中时缓存版本的详细日志记录的支持。

### 3.1.2

- 修复 Windows 上的符号链接还原问题。

### 3.1.3

- 修复防止全局设置 MYSYS 环境变量的问题 [#1329](https://github.com/actions/toolkit/pull/1329).

### 3.1.4

- 修复由于 zstd 1.5.4 发布中 `zstd --version` 输出更改而未使用 zstd 的问题。见 [#1353](https://github.com/actions/toolkit/pull/1353).

### 3.2.0

- 在缓存还原 `DownloadOptions` 中添加 `lookupOnly`。

### 3.2.1

- 更新 @azure/storage-blob 到 `v12.13.0`

### 3.2.2

- 添加新的默认缓存下载方法以提高性能并减少卡顿 [#1484](https://github.com/actions/toolkit/pull/1484)