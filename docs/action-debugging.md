# 调试
如果作业日志未提供足够详细的信息来解释作业失败的原因，还有其他选项可用于协助故障排除。

## 步骤调试日志
这是客户调试由失败的步骤引起的作业失败的主要方式。

步骤调试日志在作业执行期间和之后增加了作业日志的详细程度，以协助故障排除。

具有前缀`::debug::`的附加日志事件现在也将出现在作业日志中，这些日志事件由操作的作者和运行程序过程提供。

### 如何访问步骤调试日志
可以通过[设置密钥](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets#creating-encrypted-secrets)`ACTIONS_STEP_DEBUG`为`true`来启用此标志。

在启用此密钥时运行的所有操作将在[下载的日志](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/managing-a-workflow-run#downloading-logs)和[Web日志](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/managing-a-workflow-run#viewing-logs-to-diagnose-failures)中显示调试事件。

## 运行器诊断日志
运行器诊断日志提供了详细的日志文件，详细说明了运行器如何执行操作。

只有在认为GitHub Actions存在基础架构问题，并且希望产品团队检查日志时，才需要运行器诊断日志。

每个文件包含与该过程相对应的不同日志信息：
  * 运行器过程协调设置工作者以执行作业。
  * 工作者过程执行作业。

这些文件包含前缀`Runner_`或`Worker_`以指示日志来源。

### 如何访问运行器诊断日志
可以通过[设置密钥](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets#creating-encrypted-secrets)`ACTIONS_RUNNER_DEBUG`为`true`来启用这些日志文件。

在启用此密钥时运行的所有操作都会在[日志存档](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/managing-a-workflow-run#downloading-logs)的`runner-diagnostic-logs`文件夹中包含额外的诊断日志文件。