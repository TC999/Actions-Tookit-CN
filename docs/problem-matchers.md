# 问题匹配器

问题匹配器是一种扫描操作输出的方式，用于查找指定的正则表达式模式，并在用户界面中突出显示该信息。当检测到匹配时，将创建[GitHub注释](https://developer.github.com/v3/checks/runs/#annotations-object-1)和日志文件修饰。

## 限制

目前，GitHub Actions在工作流运行中限制注释计数。

- 每个步骤有10个警告注释，10个错误注释和10个通知注释
- 每个作业有50个注释（所有步骤的注释之和）
- 每次运行有50个注释（与作业注释分开，这些注释不是由用户创建的）

如果您的工作流可能超过这些注释计数，请考虑过滤问题匹配器暴露的日志消息（例如，通过PR涉及的文件、行或其他方式）。

## 单行匹配器

让我们考虑ESLint的紧凑输出：

```
badFile.js: line 50, col 11, Error - 'myVar' is defined but never used. (no-unused-vars)
```

我们可以在json中定义一个问题匹配器，用于检测该格式的输入：

```json
{
    "problemMatcher": [
        {
            "owner": "eslint-compact",
            "pattern": [
                {
                    "regexp": "^(.+):\\sline\\s(\\d+),\\scol\\s(\\d+),\\s(Error|Warning|Info)\\s-\\s(.+)\\s\\((.+)\\)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5,
                    "code": 6
                }
            ]
        }
    ]
}
```

问题匹配器的以下字段可用：

```
{
    owner: 可用于删除或替换问题匹配器的ID字段。 **必需**
    severity: 指示默认严重性的字段，可以是'warning'或'error'（不区分大小写）。默认为'error'
    pattern: [
        {
            regexp: 提供要与之匹配的组的正则表达式模式 **必需**
            file: 包含文件名的组号
            fromPath: 包含用于根文件的文件路径的组号（例如，项目文件）
            line: 包含行号的组号
            column: 包含列信息的组号
            severity: 包含'warning'或'error'（不区分大小写）的组号。默认为 `error`
            code: 包含错误代码的组号
            message: 包含错误消息的组号。**必需**至少一个模式必须设置消息
            loop: 是否循环，直到找不到匹配项。仅在多模式匹配器的最后一个模式上有效
        }
    ]
}
```

## 多行匹配
考虑以下输出：

```
test.js
  1:0   error  Missing "use strict" statement                 strict
  5:10  error  'addOne' is defined but never used             no-unused-vars
✖ 2 problems (2 errors, 0 warnings)
```

文件名只打印一次，但打印了多个错误行。`loop`关键字提供了一种在输出中发现多个错误的方法。

下面定义的eslint-stylish问题匹配器捕捉该输出，并从中创建两个注释。

```
{
    "problemMatcher": [
        {
            "owner": "eslint-stylish",
            "pattern": [
                {
                    // 匹配输出中的第一行
                    "regexp": "^([^\\s].*)$",
                    "file": 1
                },
                {
                    // 匹配输出中的第二和第三行
                    "regexp": "^\\s+(\\d+):(\\d+)\\s+(error|warning|info)\\s+(.*)\\s\\s+(.*)$",
                    // 文件从上面传递过来，因此我们定义剩下的组
                    "line": 1,
                    "column": 2,
                    "severity": 3,
                    "message": 4,
                    "code": 5,
                    "loop": true
                }
            ]
        }
    ]
}
```

第一个模式匹配`test.js`行并记录文件信息。此行不会在用户界面中显示。第二个模式使用`loop: true`循环处理其余行，直到找不到匹配项，并在用户界面中突出显示这些行。

请注意，模式匹配必须在连续的行上。以下不会导致任何匹配结果。

```
test.js
  无关的日志行，不感兴趣
  1:0   error  Missing "use strict" statement                 strict
  5:10  error  'addOne' is defined but never used             no-unused-vars
✖ 2 problems (2 errors, 0 warnings)
```

## 添加和删除问题匹配器

通过工具包[命令](commands.md#problem-matchers)启用和移除问题匹配器。

## 重复的问题匹配器

注册具有相同owner的两个问题匹配器将导致仅运行最后注册的问题匹配器。

## 例子

一些起始动作已经使用了问题匹配器，例如：
- [setup-node](https://github.com/actions/setup-node/tree/main/.github)
- [setup-python](https://github.com/actions/setup-python/tree/main/.github)
- [setup-go](https://github.com/actions/setup-go/tree/main/.github)
- [setup-dotnet](https://github.com/actions/setup-dotnet/tree/main/.github)

## 故障排除

### 正则表达式不匹配

在测试模式时使用ECMAScript正则表达式语法。

### 文件属性被丢弃

[启用调试日志记录](https://docs.github.com/en/actions/managing-workflow-runs/enabling-debug-logging)以确定为什么文件被丢弃。

通常发生在文件不存在或不在工作流存储库中

时。