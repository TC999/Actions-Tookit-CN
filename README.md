<p align="center">
  <img src="res/at-logo.png">
</p>

<p align="center">
  <a href="https://github.com/actions/toolkit/actions?query=workflow%3Atoolkit-unit-tests"><img alt="Toolkit unit tests status" src="https://github.com/actions/toolkit/workflows/toolkit-unit-tests/badge.svg"></a>
  <a href="https://github.com/actions/toolkit/actions?query=workflow%3Atoolkit-audit"><img alt="Toolkit audit status" src="https://github.com/actions/toolkit/workflows/toolkit-audit/badge.svg"></a>
</p>

## GitHub Actions Toolkit

GitHub操作工具包提供一组软件包，以简化创建操作的过程。

<br/>
<h3 align="center">使用<a href="https://github.com/actions/javascript-action">javascript-action模板</a>开始吧！</h3>
<br/>

## 该中文版信息（译者加）
**注意：该仓库不含源代码，请到[原版仓库](https://github.com/actions/toolkit)下载源代码！！！**
- 译者：ChatGPT3.5
- 该中文译文最后更新日期：2023年12月3日
- 如遇翻译不准确或原文档更新请提议题(issue)

## 文档目录（译者加）
- [安全](SECURITY.md)
- [贡献者公约行为准则](CODE_OF_CONDUCT.md)
- 文档
  - 架构决策记录([自述文件](docs/adrs/README.md)|[模块用法](docs/adrs/0381-glob-module.md))
  - [操作调试](docs/action-debugging.md)
  - [操作类型](docs/action-types.md)
  - [版本控制](docs/action-versioning.md)
  - [指令](docs/commands.md)
  - [使用工具包创建容器操作（空）](docs/container-action-toolkit.md)
  - [创建 Docker 操作](docs/container-action.md)
  - [使用 GitHub 上下文创建操作](docs/github-package.md)
  - [问题匹配器](docs/problem-matchers.md)
  - [代理服务器支持](docs/proxy-support.md)
- 软件包
  - artifact ([自述文件](packages/artifact/README.md)|[更新记录](packages/artifact/RELEASES.md)|[贡献](packages/artifact/CONTRIBUTIONS.md))
  - cache ([自述文件](packages/cache/README.md)|[更新记录](packages/cache/RELEASES.md))
  - core ([自述文件](packages/core/README.md)|[更新记录](packages/core/RELEASES.md))
  - exec ([自述文件](packages/exec/README.md)|[更新记录](packages/exec/RELEASES.md))
  - github ([自述文件](packages/github/README.md)|[更新记录](packages/github/RELEASES.md))
  - glob ([自述文件](packages/glob/README.md)|[更新记录](packages/glob/RELEASES.md))
  - http-client ([自述文件](packages/http-client/README.md)|[更新记录](packages/http-client/RELEASES.md))
  - io ([自述文件](packages/io/README.md)|[更新记录](packages/io/RELEASES.md))
  - tool-cache ([自述文件](packages/tool-cache/README.md)|[更新记录](packages/tool-cache/RELEASES.md))

## 软件包

:heavy_check_mark: [@actions/core](packages/core)

提供输入、输出、结果、日志、密钥和变量的函数。在[这里](packages/core)阅读更多内容

```bash
$ npm install @actions/core
```
<br/>

:runner: [@actions/exec](packages/exec)

提供执行CLI工具和处理输出的函数。在[这里](packages/exec)阅读更多内容

```bash
$ npm install @actions/exec
```
<br/>

:ice_cream: [@actions/glob](packages/glob)

提供搜索与glob模式匹配的文件的函数。在[这里](packages/glob)阅读更多内容

```bash
$ npm install @actions/glob
```
<br/>

:phone: [@actions/http-client](packages/http-client)

专为构建操作而优化的轻量级HTTP客户端。在[这里](packages/http-client)阅读更多内容

```bash
$ npm install @actions/http-client
```
<br/>

:pencil2: [@actions/io](packages/io)

提供磁盘I/O功能，如cp、mv、rmRF、which等。在[这里](packages/io)阅读更多内容

```bash
$ npm install @actions/io
```
<br/>

:hammer: [@actions/tool-cache](packages/tool-cache)

提供下载和缓存工具的函数。例如，setup-*操作。在[这里](packages/tool-cache)阅读更多内容

查看@actions/cache以缓存工作流依赖项。

```bash
$ npm install @actions/tool-cache
```
<br/>

:octocat: [@actions/github](packages/github)

提供使用当前操作运行上下文填充的Octokit客户端。在[这里](packages/github)阅读更多内容

```bash
$ npm install @actions/github
```
<br/>

:floppy_disk: [@actions/artifact](packages/artifact)

提供与操作工件交互的函数。在[这里](packages/artifact)阅读更多内容

```bash
$ npm install @actions/artifact
```
<br/>

:dart: [@actions/cache](packages/cache)

提供缓存依赖项和构建输出以提高工作流执行时间的函数。在[这里](packages/cache)阅读更多内容

```bash
$ npm install @actions/cache
```
<br/>

## 使用工具包创建操作

:question: [选择操作类型](docs/action-types.md)

概述了创建JavaScript或基于容器的操作的区别以及为什么要选择其中一种。
<br/>
<br/>

:curly_loop: [版本控制](docs/action-versioning.md)

操作是从GitHub仓库图中下载并运行的。这包含有关操作版本控制和安全发布的指南。
<br/>
<br/>

:warning: [问题匹配器](docs/problem-matchers.md)

问题匹配器是一种扫描操作输出以查找指定正则模式并在UI中突出显示该信息的方法。
<br/>
<br/>

:warning: [代理服务器支持](docs/proxy-support.md)

可以配置自托管的运行程序以在代理服务器后运行。
<br/>
<br/>

<h3><a href="https://github.com/actions/hello-world-javascript-action">Hello World JavaScript操作</a></h3>

演示如何创建一个简单的Hello World JavaScript操作。

```javascript
...
  const nameToGreet = core.getInput('who-to-greet');
  console.log(`Hello ${nameToGreet}!`);
...
```
<br/>

<h3><a href="https://github.com/actions/javascript-action">JavaScript操作演练</a></h3>

演练并使用测试、linting、工作流、发布和版本控制创建JavaScript操作的模板。

```javascript
async function run() {
  try {
    const ms = core.getInput('milliseconds');
    console.log(`Waiting ${ms} milliseconds ...`)
    ...
```
```javascript
PASS ./index.test.js
  ✓ throws invalid number
  ✓ wait 500 ms
  ✓ test runs

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
```
<br/>

<h3><a href="https://github.com/actions/typescript-action">TypeScript操作演练</a></h3>

演练创建带有编译、测试、linting、工作流、发布和版本控制的TypeScript操作。

```javascript
import * as core from '@actions/core';

async function run() {
  try {
    const ms = core.getInput('milliseconds');
    console.log(`Waiting ${ms} milliseconds ...`)
    ...
```
```javascript
PASS ./index.test.js
  ✓ throws invalid number
  ✓ wait 500 ms
  ✓ test runs

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
```
<br/>
<br/>

<h3><a href="docs/container-action.md">Docker操作演练</a></h3>

创建一个作为容器交付并使用docker运行的操作。

```docker
FROM alpine:3.10
COPY LICENSE README.md /
COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```
<br/>

<h3><a href="https://github.com/actions/container-toolkit-action">带有Octokit的Docker操作演练</a></h3>

创建一个作为容器交付并使用工具包的操作。此示例使用GitHub上下文构建Octokit客户端。

```docker
FROM node:slim
COPY . .
RUN npm install --production
ENTRYPOINT ["node", "/lib/main.js"]
```
```javascript
const myInput = core.getInput('myInput');
core.debug(`

Hello ${myInput} from inside a container`);

const context = github.context;
console.log(`We can even get context data, like the repo: ${context.repo.repo}`)
```
<br/>

## 贡献

我们欢迎贡献。请查看[如何贡献](.github/CONTRIBUTING.md)。

## 行为准则

请查看[我们的行为准则](CODE_OF_CONDUCT.md)。

## 许可证

原项目采用[MIT许可证](LICENSE.md)，中文译文如下（仅英文原版有效，中译仅作参考）：

MIT 许可证（MIT）

版权所有 2019 GitHub

特此授予任何获得此软件及相关文档文件（以下简称“软件”）副本的人免费许可，无限制地处理本软件，包括但不限于使用、复制、修改、合并、发布、分发、再许可和/或销售本软件的副本，并允许被授予该软件的人员这样做，但须符合以下条件：

在所有副本或实质部分的软件中必须包含上述版权声明和本许可声明。

本软件按原样提供，不提供任何形式的明示或暗示担保，包括但不限于适销性、特定用途适用性和非侵权性的担保。在任何情况下，作者或版权持有人均不对因使用、修改或其他方式引起的任何索赔、损害或其他责任负责，无论是合同、侵权还是其他行为的结果。