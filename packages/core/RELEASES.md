# @actions/core 发布版本

### 1.10.1
- 修复 oidc 工具中的错误消息引用 [#1511](https://github.com/actions/toolkit/pull/1511)

### 1.10.0
- `saveState` 和 `setOutput` 现在如果可用将使用环境文件 [#1178](https://github.com/actions/toolkit/pull/1178)
- `getMultilineInput` 现在默认正确修剪空白 [#1185](https://github.com/actions/toolkit/pull/1185)

### 1.9.1
- 在调用 `core.exportVariable` 时随机化分隔符

### 1.9.0
- 添加了 `toPosixPath`、`toWin32Path` 和 `toPlatformPath` 实用工具 [#1102](https://github.com/actions/toolkit/pull/1102)

### 1.8.2
- 更新到 `@actions/http-client` 的 v2.0.1 [#1087](https://github.com/actions/toolkit/pull/1087)

### 1.8.1
- 更新到 `@actions/http-client` 的 v2.0.0

### 1.8.0
- 弃用 `markdownSummary` 扩展导出，改用 `summary`
  - https://github.com/actions/toolkit/pull/1072
  - https://github.com/actions/toolkit/pull/1073

### 1.7.0
- [添加 `markdownSummary` 扩展](https://github.com/actions/toolkit/pull/1014)

### 1.6.0
- [添加 OIDC 客户端函数 `getIDToken`](https://github.com/actions/toolkit/pull/919)
- [向 `AnnotationProperties` 添加 `file` 参数](https://github.com/actions/toolkit/pull/896) 

### 1.5.0
- [添加支持通知注释和更多注释字段](https://github.com/actions/toolkit/pull/855)

### 1.4.0
- [添加 `getMultilineInput` 函数](https://github.com/actions/toolkit/pull/829)

### 1.3.0
- [向 `getInput` 添加 `trimWhitespace` 选项](https://github.com/actions/toolkit/pull/802)
- [添加 `getBooleanInput` 函数](https://github.com/actions/toolkit/pull/725)

### 1.2.7
- [为 `set-output` 添加前置换行符](https://github.com/actions/toolkit/pull/772)

### 1.2.6
- [更新 `exportVariable` 和 `addPath` 以使用环境文件](https://github.com/actions/toolkit/pull/571)

### 1.2.5
- [正确捆绑包许可文件](https://github.com/actions/toolkit/pull/548)

### 1.2.4
- [在接受非字符串命令输入时更加宽松](https://github.com/actions/toolkit/pull/405)
- [添加 Echo 命令](https://github.com/actions/toolkit/pull/411)

### 1.2.3

- [IsDebug 日志记录](README.md#logging)

### 1.2.2

- [修复 runner 命令的转义](https://github.com/actions/toolkit/pull/302)

### 1.2.1

- [从命令中删除尾随逗号](https://github.com/actions/toolkit/pull/263)
- [在 package.json 中添加 "types"](https://github.com/actions/toolkit/pull/221)

### 1.2.0

- 为包装任务（在最终运行的入口点中运行的后作业）添加了 `saveState` 和 `getState` 函数

### 1.1.3 

- 添加了 `setSecret` 以向 runner 注册要从日志中隐藏的秘密
- 在从产品中得到澄清后删除了未实现且从未工作的 `exportSecret`。

### 1.1.1

- 添加对具有多个空格的操作输入变量的支持 [#127](https://github.com/actions/toolkit/issues/127)
- 切换 ## 命令到 :: 命令（应该没有明显的影响） [#110](https://github.com/actions/toolkit/pull/110)

### 1.1.0

- 添加了 `group` 和 `endgroup` 的辅助函数 [#98](https://github.com/actions/toolkit/pull/98)

### 1.0.0

- 初始发布