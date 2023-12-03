# `@actions/glob`

## 用法

### 基本用法

您可以使用此软件包搜索与 glob 模式匹配的文件。

允许相对路径和绝对路径。相对路径以当前工作目录为根。

```js
const glob = require('@actions/glob');

const patterns = ['**/tar.gz', '**/tar.bz']
const globber = await glob.create(patterns.join('\n'))
const files = await globber.glob()
```

### 不跟随符号链接

```js
const glob = require('@actions/glob');

const globber = await glob.create('**', {followSymbolicLinks: false})
const files = await globber.glob()
```

### 迭代器

在处理大量结果时，考虑在返回的结果中进行迭代：

```js
const glob = require('@actions/glob');

const globber = await glob.create('**')
for await (const file of globber.globGenerator()) {
  console.log(file)
}
```

## 推荐的操作输入

Glob 默认情况下会跟随符号链接。通常情况下，跟随符号链接是合适的，除非删除文件。

用户可能希望出于其他原因选择不跟随符号链接。例如，
过多的符号链接可能导致看起来有很多很多文件的现象
并减缓搜索速度。

当一个操作允许用户指定输入模式时，通常建议
允许用户选择是否跟随符号链接。

`action.yml` 的片段：

```yaml
inputs:
  files:
    description: 'Files to print'
    required: true
  follow-symbolic-links:
    description: 'Indicates whether to follow symbolic links'
    default: true
```

以及相应的 toolkit 使用：

```js
const core = require('@actions/core')
const glob = require('@actions/glob')

const globOptions = {
  followSymbolicLinks: core.getInput('follow-symbolic-links').toUpper() !== 'FALSE'
}
const globber = glob.create(core.getInput('files'), globOptions)
for await (const file of globber.globGenerator()) {
  console.log(file)
}
```

## 模式

### Glob 行为

支持模式 `*`, `?`, `[...]`, `**` (globstar)。

具有以下行为：
- 以 `.` 开头的文件名可能包含在结果中
- 在 Windows 上不区分大小写
- 在 Windows 上同时支持目录分隔符 `/` 和 `\`

### 波浪线扩展

支持基本的波浪线扩展，仅用于当前用户 HOME 替换。

例如：
- `~` 可能扩展为 /Users/johndoe
- `~/foo` 可能扩展为 /Users/johndoe/foo

### 注释

以 `#` 开头的模式被视为注释。

### 排除模式

前导 `!` 改变包含模式的含义为排除。

多个前导 `!` 反转含义。

### 转义

将特殊字符包装在 `[]` 中可用于转义文件名中的字面 glob 字符。
例如，文字文件名 `hello[a-z]` 可以转义为 `hello[[]a-z]`。在 Linux/macOS 上，`\` 也被视为转义字符。