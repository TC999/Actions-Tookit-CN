# `@actions/github`

> 一个经过润色的 Octokit 客户端。

## 用法

返回一个经过身份验证的 Octokit 客户端，该客户端遵循机器的 [代理设置](https://help.github.com/en/actions/hosting-your-own-runners/using-a-proxy-server-with-self-hosted-runners) 并正确设置 GHES 基础 URL。有关 API，请参见 https://octokit.github.io/rest.js。

```js
const github = require('@actions/github');
const core = require('@actions/core');

async function run() {
    // 这应该是一个具有对您的存储库的访问权限的令牌，作为机密密钥域传递。
    // YML 工作流将需要使用 GitHub 秘密令牌设置 myToken
    // myToken: ${{ secrets.GITHUB_TOKEN }}
    // https://help.github.com/en/actions/automating-your-workflow-with-github-actions/authenticating-with-the-github_token-secret
    const myToken = core.getInput('myToken');

    const octokit = github.getOctokit(myToken)

    // 您还可以将其他选项作为 getOctokit 的第二个参数传递
    // const octokit = github.getOctokit(myToken, {userAgent: "MyActionVersion1"});

    const { data: pullRequest } = await octokit.rest.pulls.get({
        owner: 'octokit',
        repo: 'rest.js',
        pull_number: 123,
        mediaType: {
          format: 'diff'
        }
    });

    console.log(pullRequest);
}

run();
```

您还可以进行 GraphQL 请求。有关 API，请参见 https://github.com/octokit/graphql.js。

```js
const result = await octokit.graphql(query, variables);
```

最后，您可以获取当前操作的上下文：

```js
const github = require('@actions/github');

const context = github.context;

const newIssue = await octokit.rest.issues.create({
  ...context.repo,
  title: 'New issue!',
  body: 'Hello Universe!'
});
```

## Webhook 负载 TypeScript 定义

npm 模块 `@octokit/webhooks-definitions` 提供了响应负载的类型定义。您可以将负载转换为这些类型以获取更好的类型信息。

首先，安装 npm 模块 `npm install @octokit/webhooks-definitions`

然后，根据 eventName 断言类型
```ts
import * as core from '@actions/core'
import * as github from '@actions/github'
import {PushEvent} from '@octokit/webhooks-definitions/schema'

if (github.context.eventName === 'push') {
  const pushPayload = github.context.payload as PushEvent
  core.info(`The head commit is: ${pushPayload.head_commit}`)
}
```

## 扩展 Octokit 实例
`@octokit/core` 现在支持 [插件体系结构](https://github.com/octokit/core.js#plugins)。您可以使用插件扩展 GitHub 实例。

例如，使用 `@octokit/plugin-enterprise-server`，您现在可以在 GHES 实例上访问企业管理员 API。

```ts
import { GitHub, getOctokitOptions } from '@actions/github/lib/utils'
import { enterpriseServer220Admin } from '@octokit/plugin-enterprise-server'

const octokit = GitHub.plugin(enterpriseServer220Admin)
// 或者覆盖一些默认值
// const octokit = GitHub.plugin(enterpriseServer220Admin).defaults({userAgent: "MyNewUserAgent"})

const myToken = core.getInput('myToken');
const myOctokit = new octokit(getOctokitOptions(token))
// 创建一个新用户
myOctokit.rest.enterpriseAdmin.createUser({
  login: "testuser",
  email: "testuser@test.com",
});
```