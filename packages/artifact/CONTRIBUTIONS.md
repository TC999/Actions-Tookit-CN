# 贡献

此包在 [upload-artifact](https://github.com/actions/upload-artifact) 和 [download-artifact](https://github.com/actions/download-artifact) 的 v2+ 版本中被内部使用。其他操作也可以使用此包与工件进行交互。对此包的任何更改或更新将传播到这些操作，因此重要的是要对主要更改或更新进行适当测试。

与工件操作相关的任何问题或功能请求都应填写在相应的存储库中。

每次对工件包进行更改时，一定范围的单元测试会作为每个 PR 的一部分运行。对于小的贡献和修复，它们应该足够了。

如果进行重大更改，有一些情景应该进行测试。

- 上传非常大的工件（大工件使用 gzip 进行压缩，因此必须测试压缩/解压缩）
- 上传包含许多小文件的工件（每个文件都使用自己的 HTTP 调用上传，可以预期超时和非成功的 HTTP 响应，因此必须正确处理）
- 使用自托管运行器上传工件（由于额外的延迟，上传和下载的行为不同）
- 下载单个工件（大和小，如果工件中包含许多小文件，则可以预期超时和非成功的 HTTP 响应）
- 一次性下载所有工件

大的架构更改可能会影响上传/下载性能，因此重要的是单独运行额外的测试。我们要求任何大的贡献/更改都有额外的详细测试，以便我们可以验证性能和可能的回归。

由于某些环境变量，如 `ACTIONS_RUNTIME_URL` 仅在操作的上下文中而不是 shell 脚本的上下文中可用，因此不可能在此存储库的 PR 的一部分运行工件的端到端测试。这些环境变量是为了进行必要的 API 调用而需要的。

# 测试

测试更改的一种简单方法是分叉工件操作并使用 `npm link` 测试更改。

1. 分叉 [upload-artifact](https://github.com/actions/upload-artifact) 和 [download-artifact](https://github.com/actions/download-artifact) 存储库
2. 在本地克隆分叉
3. 使用对工具包存储库的本地更改，确保运行 `tsc` 时没有错误后，输入 `npm link`
4. 在本地克隆的分叉中，输入 `npm link @actions/artifact`
5. 使用 `tsc` 和 `npm run release` 为本地分叉创建一个新的发布（这将使用 `@vercel/ncc` 创建一个新的 `dist/index.js` 文件）
6. 提交并推送本地更改，然后您将能够使用分叉的操作测试更改