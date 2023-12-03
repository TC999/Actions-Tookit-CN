ADR 381：`glob` 模块

**日期**：2019年12月5日

**状态**：已接受

## 背景

本ADR提议向工具包添加一个`glob`函数。

第一方操作应具有一致的全局体验。

与工件上传/下载v2相关。

## 决定

### 新模块

创建一个新模块 `@actions/glob`，可以以自己的节奏进行版本化，不与 `@actions/io` 绑定。

### 签名

```js
/**
 * 从模式构建全局搜索器
 *
 * @param patterns 由换行符分隔的模式
 * @param options 全局选项
 */
export function create(
  patterns: string,
  options?: GlobOptions
): Promise<Globber> {}

/**
 * 用于匹配文件和目录
 */
export interface Globber {
  /**
   * 返回每个模式的第一个全局段之前的搜索路径。
   * 过滤掉其他路径的重复项和后代。
   *
   * 示例1：模式 `/foo/*` 和 `/bar/*` 返回 `/foo` 和 `/bar`。
   *
   * 示例2：模式 `/foo/*` 和 `/foo/bar/*` 返回 `/foo`。
   */
  getSearchPaths(): string[]

  /**
   * 返回与全局模式匹配的文件和目录。
   *
   * 不保证结果的顺序。
   */
  glob(): Promise<string[]>

  /**
   * 返回与全局模式匹配的文件和目录。
   *
   * 不保证结果的顺序。
   */
  globGenerator(): AsyncGenerator<string, void>
}

/**
 * 用于控制全局行为的选项
 */
export interface GlobOptions {
  /**
   * 指示是否跟随符号链接。通常在删除文件时应设置为false。
   *
   * @default true
   */
  followSymbolicLinks?: boolean

  /**
   * 指示匹配全局模式的目录是否应隐式导致匹配所有后代路径。
   *
   * 例如，给定目录`my-dir`，以下全局模式将产生相同的结果：`my-dir/**`，`my-dir/`，`my-dir`
   *
   * @default true
   */
  implicitDescendants?: boolean

  /**
   * 指示是否应忽略并从结果集中省略损坏的符号链接。否则将引发错误。
   *
   * @default true
   */
  omitBrokenSymbolicLinks?: boolean
}
```

### 工具包用法

例如，不要跟随符号链接：

```js
const patterns = core.getInput('path')
const globber = glob.create(patterns, {followSymbolicLinks: false})
const files = globber.glob()
```

例如，迭代器：

```js
const patterns = core.getInput('path')
const globber = glob.create(patterns)
for await (const file of this.globGenerator()) {
  console.log(file)
}
```

### 操作用法

操作默认应遵循符号链接。

用户可以选择退出。

例如：

```yaml
jobs:
  build:
    steps:
      - uses: actions/upload-artifact@v1
        with:
          path: |
            **/*.tar.gz
            **/*.pkg
          follow-symbolic-links: false    # 选择退出，默认为true
```

### HashFiles 函数

哈希文件默认不应遵循符号链接。

用户可以通过指定标志 `--follow-symbolic-links` 进行选择性选择。

例如：

```yaml
jobs:
  build:
    steps:
      - uses: actions/cache@v1
        with:
          hash: ${{ hashFiles('--follow-symbolic-links', '**/package-lock.json') }}
```

### 全局行为

支持模式 `*`、`?`、`[...]`、`**`（全局星号）。

具有以下行为：

- 以 `.` 开头的文件名可能包含在结果中
- 在Windows上不区分大小写
- Windows上同时支持目录分隔符 `/` 和 `\`

注意：
- 有关Bash全局模式的更多信息，请参阅[此处](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html#Pattern-Matching)。
- 有关Bash全局选项的更多信息，请参阅[此处](https://www.gnu.org/software/bash/manual/html_node/The-Shopt-Builtin.html)。

### 波浪线扩展

支持基本的波浪线扩展，仅用于当前用户HOME替换。

例如，在macOS上：
- `~` 可能扩展为 `/Users/johndoe`
- `~/foo` 可能扩展为 `/Users/johndoe/foo`

注意：
- 有关Bash波浪线扩展的更多信息，请参阅[此处](https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html)。
- 不支持所有其他形式的波浪线扩展。
- 使用 `os.homedir()` 解析HOME路径

### 根和规范化路径

未根模式将使用当前工作目录在搜索之前被根化。此外，在搜索之前，搜索路径将被规范化（去除相对路径，Windows上的斜杠规范化，额外的斜杠删除）。

这两个副作用是：
1. 总是返回根化和规范化的路径
2. 模式 `**` 将在结果中包含工作目录

这些副作用与Bash行为不同。而Bash设计为是一个Shell，我们设计的是一个API。这个决定旨在提高API结果的可预测性。

注意：
- 在Bash中，当模式是相对的时，结果是没有根的。
- 在Bash中，结果不是规范化的。例如，从 `./*` 得到的结果可能是： `./foo ./bar`
- 在Bash中，模式 `**` 的结果不包含工作目录。但是来自 `/foo/**` 的结果将包含目录 `/foo`。另外，来自 `foo/**` 的结果将包含目录 `foo`。

## 评论

以 `#` 开头的模式被视

为注释。

## 排除模式

前导的 `!` 更改包含模式的含义以排除。

注意：
- 多个前导的 `!` 会翻转含义。

## 转义

在 `[]` 中包装特殊字符可用于在文件名中转义文字全局字符。例如，文字文件名 `hello[a-z]` 可以被转义为 `hello[[]a-z]`。

在Linux/macOS上，`\` 也被视为转义字符。

## 结果

- 发布新模块 `@actions/glob`
- 为模块发布文档（从 `./README.md` 添加链接到新文档 `./packages/glob/README.md`）