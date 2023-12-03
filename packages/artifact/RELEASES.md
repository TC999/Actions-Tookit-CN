# @actions/artifact 发行版

### 0.1.0

- 初始发布

### 0.2.0

- 修复 TCP 连接未关闭的问题
- 使用 GZip 文件压缩以加速下载
- 改进的日志记录和输出
- 额外的文档

### 0.3.0

- 修复下载工件时的 gzip 解压缩问题
- 支持处理 429 响应代码
- 在处理空文件时改进下载体验
- 在遇到可重试状态代码时使用指数退避
- 如果已达到存储配额，提供更清晰的错误消息
- 在下载工件期间改进日志记录和输出

### 0.3.1

- 修复确保在上传工件期间正确删除临时 gzip 文件的问题
- 在上传时删除空格作为禁止字符

### 0.3.2

- 修复在重试时确保 readstreams 正确重置的问题

### 0.3.3

- 将上传时的块大小从 4MB 增加到 8MB
- 在 API 调用期间改进用户代理字符串，以帮助内部诊断问题

### 0.3.5

- 在收到 413 响应时进行重试

### 0.4.0

- 添加在工件上指定自定义保留期的选项

### 0.4.1

- 更新到最新的 @actions/core 版本

### 0.4.2

- 在遇到部分工件下载时提高重试能力

### 0.5.0

- 如果遇到错误，改进工件上传和下载期间所有 http 调用的重试能力

### 0.5.1

- 将 @actions/http-client 升级到版本 1.0.11 以修复与工件上传和下载相关的代理问题

### 0.5.2

- 将 HTTP 500 添加为工件上传和下载的可重试状态代码。

### 0.6.0

- 支持从命名管道上传 [#748](https://github.com/actions/toolkit/pull/748)
- 修复下载所有工件时百分比值大于 100% 的问题 [#889](https://github.com/actions/toolkit/pull/889)
- 在工件上传期间改进日志记录和输出 [#949](https://github.com/actions/toolkit/pull/949)
- 对于在上传期间不允许的某些无效字符，客户端端验证的改进：[#951](https://github.com/actions/toolkit/pull/951)
- 通过免除 gzip 压缩，加快某些类型的大文件上传速度 [#956](https://github.com/actions/toolkit/pull/956)
- 在处理分块上传时提供更详细的日志记录 [#957](https://github.com/actions/toolkit/pull/957)

### 0.6.1

- 修复在 Windows 上上传 0 字节文件失败的问题 [#962](https://github.com/actions/toolkit/pull/962)

### 1.0.0

- 在 `package-lock.json` 中将 `lockfileVersion` 更新为 `v2` [#1009](https://github.com/actions/toolkit/pull/1009)

### 1.0.1

- 更新到 `@actions/http-client` 的 v2.0.0 版本

### 1.0.2

- 更新到 `@actions/http-client` 的 v2.0.1 版本 [#1087](https://github.com/actions/toolkit/pull/1087)

### 1.1.0

- 在上传时添加 `x-actions-results-crc64` 和 `x-actions-results-md5` 校验头 [#1063](https://github.com/actions/toolkit/pull/1063)

### 1.1.1

- 在 Node16 中修复一个 bug，当 HTTP 下载完成得太快（<1ms，例如当它被模拟时），我们尝试删除尚未创建的临时文件的问题 [#1278](https://github.com/actions/toolkit/pull/1278/commits/b9de68a590daf37c6747e38d3cb4f1dd2cfb791c)