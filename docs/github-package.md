# 使用 GitHub 上下文创建操作

## 目标

在这个演示中，我们将学习如何使用 GitHub 上下文数据构建一个基本的操作，以在用户打开问题或 PR 时向其问候。在这个过程中，我们将探讨如何访问这个上下文以及如何对 GitHub API 进行身份验证请求。

请注意，可以在 https://github.com/damccorm/issue-greeter 找到这个操作的完整版本。

## 先决条件

这个演示假定您已经完成了基本的 [JavaScript 操作演示](https://github.com/actions/javascript-action)，并且已经设置了一个基本的操作。如果没有，请先进行该演示。

## 安装依赖项

我们需要的所有依赖项都应该在此库的核心和 GitHub 包中打包提供。要安装，请在操作中运行以下命令：

`npm install @actions/core && npm install @actions/github`

## 元数据

接下来，我们将需要一个欢迎消息和一个作为输入的存储库令牌。请记住，输入是在 `action.yml` 元数据文件中定义的 - 更新您的 `action.yml` 文件以定义 `welcome-message` 和 `repo-token` 作为输入。

```yaml
name: "Welcome"
description: "A basic welcome action"
author: "GitHub"
inputs:
  welcome-message:
    description: "Message to display when a user opens an issue or PR"
    default: "Thanks for opening an issue! Make sure you've followed CONTRIBUTING.md"
  repo-token:
    description: "Token for the repo. Can be passed in using {{ secrets.GITHUB_TOKEN }}"
    required: true
runs:
  using: "node12"
  main: "lib/main.js"
```

## 操作逻辑

现在，我们已经安装了我们的依赖项并定义了我们的输入，我们准备在 `src/main.ts` 中开始编写操作逻辑！为了清晰起见，我们将构造我们的操作如下：

```ts
import * as core from '@actions/core';
import * as github from '@actions/github';

export async function run() {
    try {
    const welcomeMessage: string = core.getInput('welcome-message');
    // TODO - 获取上下文数据

    // TODO - 向 GitHub API 发送评论问题的请求 
    }
    catch (error) {
      core.setFailed(error.message);
      throw error;
    }
}

run();
```

### 获取上下文数据

出于这个演示的目的，我们将需要以下上下文数据的片段：

- 操作正在运行的存储库的名称
- 该存储库的组织/所有者
- 已打开的问题的编号

幸运的是，GitHub 包通过 [一个单一的方便函数](https://github.com/actions/toolkit/blob/ac007c06984bc483fae2ba649788dfc858bc6a8b/packages/github/src/context.ts#L34) 提供了所有这些信息，所以我们可以简单地这样做：

`const issue: {owner: string; repo: string; number: number} = github.context.issue;`

上下文对象还包含许多易于访问的属性，以及对完整的 [GitHub 负载](https://developer.github.com/v3/activity/events/types/) 的简便访问。我们可以使用这个来检查并确保我们确实看到的是最近打开的问题（而不是其他事物，比如对现有问题的评论）：

```ts
if (github.context.payload.action !== 'opened') {
  console.log('No issue or PR was opened, skipping');
  return;
}
```

现在我们的整个 `src/main.ts` 文件看起来像这样：

```ts
import * as core from '@actions/core';
import * as github from '@actions/github';

export async function run() {
    try {
    const welcomeMessage: string = core.getInput('welcome-message', {required: true});
    const repoToken: string = core.getInput('repo-token', {required: true});
    const issue: {owner: string; repo: string; number: number} = github.context.issue;

    if (github.context.payload.action !== 'opened') {
      console.log('No issue or pull request was opened, skipping');
      return;
    }

    // TODO - 向 GitHub API 发送评论问题的请求 
    }
    catch (error) {
      core.setFailed(error.message);
      throw error;
    }
}

run();
```

### 向 GitHub API 发送请求

既然我们有了上下文数据，我们就可以使用 [Octokit REST 客户端](https://github.com/octokit/rest.js) 向 GitHub API 发送请求了。REST 客户端提供了许多简便的功能，包括一个用于向问题/PR添加评论的功能（问题和PR在Octokit客户端中被视为一个概念）：

```ts
const client: github.GitHub = new github.GitHub(repoToken);
await client.issues.createComment({
  owner: issue.owner,
  repo: issue.repo,
  issue_number: issue.number,
  body: welcomeMessage
});
```

有关客户端的更多文档，请访问 [Octokit REST 文档](https://octokit.github.io/rest.js/)。现在我们的操作代码应该已经完成了：

```ts
import * as core from '@actions/core';
import * as github from '@actions/github';

export async function run() {
    try {
    const welcomeMessage: string = core.getInput('welcome-message', {required: true});
    const repoToken: string = core.getInput('repo-token', {required: true});
    const issue: {owner: string; repo: string; number: number} = github.context.issue;

    if (github.context.payload.action !== 'opened') {
      console.log('No issue or pull request was opened, skipping');
      return;
    }

    const client: github.GitHub = new github.GitHub(repoToken);
    await client.issues.createComment({
      owner: issue.owner,
      repo: issue.repo,
      issue_number: issue.number,
      body: welcomeMessage
    });
    }
    catch (error) {
      core.setFailed(error.message);
      throw error;
    }
}

run();
```

## 为您的操作编写单元测试

接下来，我们将使用 jest 为我们的操作编写一个基本的单元测试。如果您按照 [JavaScript 演示](https://github.com/actions/javascript-action) 进行了操作

，那么当调用 `npm test` 时，您应该有一个运行测试的 `__tests__/main.test.ts` 文件。我们将从一个测试开始：

```ts
const nock = require('nock');
const path = require('path');

describe('action test suite', () => {
  it('It posts a comment on an opened issue', async () => {
    // TODO
  });
});
```

出于本演示的目的，我们将专注于填充此测试，并将其余的测试覆盖留给读者作为练习。

### 模拟输入

首先，我们要确保我们可以模拟我们的输入（欢迎消息和存储库令牌）。操作通过填充 `process.env.INPUT_${input name in all caps}` 来处理输入，所以我们可以简单地设置这些环境变量来模拟它们：

```ts
const nock = require('nock');
const path = require('path');

describe('action test suite', () => {
  it('It posts a comment on an opened issue', async () => {
    const welcomeMessage = 'hello';
    const repoToken = 'token';
    process.env['INPUT_WELCOME-MESSAGE'] = welcomeMessage;
    process.env['INPUT_REPO-TOKEN'] = repoToken;

    // TODO
  });
});
```

### 模拟 GitHub 上下文

模拟 GitHub 上下文相对简单。由于大部分上下文仅仅是由环境变量填充的，您只需设置 [这里](https://github.com/actions/toolkit/blob/ac007c06984bc483fae2ba649788dfc858bc6a8b/packages/github/src/context.ts#L23) 定义的相应环境变量，并测试它是否在该环境中起作用。在这种情况下，我们可以设置我们的测试如下：

```ts
const nock = require('nock');
const path = require('path');

describe('action test suite', () => {
  it('It posts a comment on an opened issue', async () => {
    const welcomeMessage = 'hello';
    const repoToken = 'token';
    process.env['INPUT_WELCOME-MESSAGE'] = welcomeMessage;
    process.env['INPUT_REPO-TOKEN'] = repoToken;

    process.env['GITHUB_REPOSITORY'] = 'foo/bar';
    process.env['GITHUB_EVENT_PATH'] = path.join(__dirname, 'payload.json');

    // TODO
  });
});
```

注意，负载是从 GITHUB_EVENT_PATH 加载的。由于我们将其设置为 `path.join(__dirname, 'payload.json')`，我们需要在那里保存我们的负载。对于此测试的目的，我们可以简单地将以下内容保存到 `__tests__/payload.json`：

```json
{
    "issue": {
        "number": 10
    },
    "action": "opened"
}
```

现在，调用 `github.context.issue` 应该返回 `{owner: foo, repo: bar, number: 10}`，而 `github.context.payload.action` 应该设置为 'opened'

> 这里的一个重要细节是，由于 GitHub 上下文在需要时加载这些环境变量，您应该在要求您的操作之前设置它们。在大多数情况下，这意味着您需要在每个测试中重新要求您的操作。如果这是个问题，您可以通过使用 jest（或您选择的任何框架）直接模拟类来避免这个问题。

### 模拟 Octokit 客户端

要模拟客户端调用，我们建议使用 [nock](https://github.com/nock/nock)，它允许您模拟客户端进行的 http 请求。首先，使用 `npm install nock --save-dev` 安装 nock。

对于这个测试，我们期望以下调用：

```ts
client.issues.createComment({
  owner: 'foo',
  repo: 'bar',
  issue_number: 10,
  body: 'you posted your first issue'
});
```

从 [GitHub 端点文档](https://developer.github.com/v3/issues/comments/#create-a-comment) 中，我们期望这会在 `https://api.github.com/repos/foo/bar/issues/10/comments` 上发出一个带有 `{"body":"hello"}` 主体的 POST 请求

我们可以用以下方式模拟：

```ts
const nock = require('nock');
const path = require('path');

describe('action test suite', () => {
  it('It posts a comment on an opened issue', async () => {
    const welcomeMessage = 'hello';
    const repoToken = 'token';
    process.env['INPUT_WELCOME-MESSAGE'] = welcomeMessage;
    process.env['INPUT_REPO-TOKEN'] = repoToken;

    process.env['GITHUB_REPOSITORY'] = 'foo/bar';
    process.env['GITHUB_EVENT_PATH'] = path.join(__dirname, 'payload.json');

    nock('https://api.github.com')
      .persist()
      .post('/repos/foo/bar/issues/10/comments', '{\"body\":\"hello\"}')
      .reply(200);
      
    const main = require('../src/main');

    await main.run();
  });
});
```

如果 url 或主体与传递给 nock 函数的参数不完全匹配，则此测试将失败。现在我们可以运行 `npm test`，测试应该成功。

## 构建和发布

现在我们已经编写并对我们的操作进行了单元测试，我们可以使用 `npm run build` 构建我们的操作，并将其推送到可以由工作流程使用的 repo 中。有关对您的操作进行版本控制的更多信息，请参阅 [我们的版本控制文档](./action-versioning.md)。

## 下一步

如果您有兴趣进一步构建此操作，请尝试将操作扩展为仅在用户第一次提出问题时运行。参考我们的 [first-contribution 操作](https://github.com/actions/first-interaction) 以获取灵感。