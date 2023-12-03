# :: 命令

[核心工具包包](https://github.com/actions/toolkit/tree/main/packages/core) 提供了许多方便的函数，用于在操作之间设置结果、记录日志、注册密钥，并在操作之间导出变量。然而，有时在脚本或其他工具中执行这些操作会很有用。

为了实现这一点，我们提供了一种特殊的 `::` 语法，如果将其记录到 `stdout` 的新行上，将允许运行时对您的命令执行特殊行为。以下命令都受支持：

### 设置输出

要为步骤设置输出，请使用 `::set-output`：

```sh
echo "::set-output name=FOO::BAR"
```

在您的 Yaml 中运行 `steps.[step-id].outputs.FOO` 现在将得到 `BAR`

```yaml
steps:
  - name: 设置值
    id: step_one
    run: echo "::set-output name=FOO::BAR"
  - name: 使用它
    run: echo ${{ steps.step_one.outputs.FOO }}
```

这被核心 `setOutput` 方法包装：

```javascript
export function setOutput(name: string, value: string): void {}
```

### 注册密钥

如果脚本或操作在运行时执行工作以创建密钥，则可以将其注册到运行时以在日志中掩码。

要在日志中掩码一个值，请使用 `::add-mask`：

```sh
echo "::add-mask::mysecretvalue"
```

这由核心 `setSecret` 方法包装

```javascript
function setSecret(secret: string): void {}
```

现在，包含 `BAR` 的未来日志将被掩码。例如，运行 `echo "Hello FOO BAR World"` 现在将打印 `Hello FOO **** World`。

**警告** `add-mask` 和 `setSecret` 命令仅支持单行
或已转义的多行密钥。`@actions/core`
`setSecret` 将默认转义您提供的字符串。当提供一个已转义的
多行字符串时，整个字符串及其每一行
将单独掩码。例如，可以使用以下命令掩码 `first\nsecond\r\nthird`：

```sh
echo "::add-mask::first%0Asecond%0D%0Athird"
```

这将掩码 `first%0Asecond%0D%0Athird`，`first`，`second` 和 `third`。

**警告** 如果可能，请勿掩码短值，这可能会使您的输出变得不可读（以及未来步骤的输出）。例如，如果掩码字母 `l`，运行 `echo "Hello FOO BAR World"` 现在将打印 `He*********o FOO BAR Wor****d`

### 分组和取消分组日志行

发出带有标题的组将指示日志创建可折叠区域，直到下一个 `endgroup` 命令。

```bash
echo "::group::my title"   
echo "::endgroup::"
```

这由核心方法包装：

```javascript
function startGroup(name: string): void {}
function endGroup(): void {}
```

### 问题匹配器

问题匹配器可用于扫描构建的输出，以自动显示与提供的模式匹配的行。必须提供到 .json 问题匹配器的文件路径。有关如何定义问题匹配器的更多信息，请参见[问题匹配器](problem-matchers.md)。

```bash
echo "::add-matcher::eslint-compact-problem-matcher.json"   
echo "::remove-matcher owner=eslint-compact::"
```

`add-matcher` 接受指向问题匹配器文件的路径
`remove-matcher` 通过所有者删除问题匹配器

### 保存状态

保存状态到稍后可在主操作或后操作中使用的环境变量。

```bash
echo "::save-state name=FOO::foovalue"
```

因为 `save-state` 将字符串 `STATE_` 添加到名称的前面，所以环境变量 `STATE_FOO` 将可用于在主操作或后操作中使用。有关更多信息，请参见[将值发送到前后操作](https://help.github.com/en/actions/reference/workflow-commands-for-github-actions#sending-values-to-the-pre-and-post-actions)。

### 日志级别

有几个命令可发出不同级别的日志输出：

| 日志级别 | 示例用法 |
|---|---|
| [debug](action-debugging.md)  | `echo "::debug::My debug message"` |
| notice | `echo "::notice::My notice message"` |
| warning | `echo "::warning::My warning message"` |
| error | `echo "::error::My error message"` |

有关其他语法选项，请参阅[工作流命令文档](https://docs.github.com/en/actions/reference/workflow-commands-for-github-actions#setting-a-debug-message)。

### 命令回显

默认情况下，仅当[启用了步骤调试](./action-debugging.md#How-to-Access-Step-Debug-Logs)时，才会将

命令回显到 stdout。

您可以通过使用 `echo` 命令为当前步骤启用或禁用这个功能。

```bash
echo "::echo::on"
```

您还可以禁用回显。

```bash
echo "::echo::off"
```

这由核心方法包装：

```javascript
function setCommandEcho(enabled: boolean): void {}
```

`add-mask`、`debug`、`warning` 和 `error` 命令不支持回显。

### 命令提示符

CMD 在回显时使用 `"` 字符时与其他 shell 处理不同。在 CMD 中，应删除上述代码片段中的 `"` 字符，以便正确处理。例如，设置输出命令将是：

```cmd
echo ::set-output name=FOO::BAR
```

## 环境文件

在工作流执行期间，运行时生成可以用于执行某些操作的临时文件。这些文件的路径通过环境变量公开。在写入这些文件时，您将需要使用 `utf-8` 编码，以确保正确处理命令。可以使用换行符将多个命令写入同一文件。

### 设置环境变量

要为将来的处理步骤设置环境变量，请写入位于 `GITHUB_ENV` 处的文件或使用等效的 `actions/core` 函数

```sh
echo "FOO=BAR" >> $GITHUB_ENV
```

在将来的步骤中运行 `$FOO` 现在将返回 `BAR`

对于多行字符串，您可以使用带有选择的分隔符的 heredoc 样式语法。在下面的示例中，我们使用 `EOF`。

```
steps:
  - name: 设置值
    id: step_one
    run: |
        echo 'JSON_RESPONSE<<EOF' >> $GITHUB_ENV
        curl https://httpbin.org/json >> $GITHUB_ENV
        echo 'EOF' >> $GITHUB_ENV
```

这将把 `JSON_RESPONSE` 环境变量的值设置为 curl 响应的值。

heredoc 样式的预期语法为：

```
{VARIABLE_NAME}<<{DELIMETER}
{VARIABLE_VALUE}
{DELIMETER}
```

这由核心 `exportVariable` 方法包装，该方法为将来的步骤设置，但也更新此步骤的变量。

```javascript
export function exportVariable(name: string, val: string): void {}
```

### PATH 操作

要将字符串前置到 PATH，请写入位于 `GITHUB_PATH` 处的文件或使用等效的 `actions/core` 函数

```sh
echo "/Users/test/.nvm/versions/node/v12.18.3/bin" >> $GITHUB_PATH
```

在将来的步骤中运行 `$PATH` 现在将返回 `/Users/test/.nvm/versions/node/v12.18.3/bin:{Previous Path}`;

这由核心 `addPath` 方法包装：

```javascript
export function addPath(inputPath: string): void {}
```

### Powershell

Powershell 默认情况下不使用 UTF8。确保以正确的编码写入。例如，要设置路径：

```
steps:
  - run: echo "mypath" | Out-File -FilePath $env:GITHUB_PATH -Encoding utf8 -Append
```
