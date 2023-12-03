# `@actions/cache`

> 用于缓存依赖项和构建输出以提高工作流执行时间的必要函数。

请参阅 ["缓存依赖项以加速工作流程"](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows) 了解缓存的工作原理。

请注意，GitHub 将删除任何在超过 7 天未访问的缓存条目。您可以存储的缓存数量没有限制，但存储库中所有缓存的总大小限制为 10 GB。如果超过此限制，GitHub 将保存您的缓存，但将开始逐出缓存，直到总大小小于 10 GB。

## 用法

此包由我们的第一方缓存操作的 v2+ 版本使用。您可以在缓存存储库中找到示例实现 [here](https://github.com/actions/cache)。

#### 保存缓存

使用提供的 `key` 保存包含 `paths` 中文件的缓存。如果安装了 zstd，则使用 zstandard 压缩算法对文件进行压缩，否则使用 gzip。如果成功保存缓存，则函数返回缓存 id，如果缓存上传失败，则抛出错误。

```js
const cache = require('@actions/cache');
const paths = [
    'node_modules',
    'packages/*/node_modules/'
]
const key = 'npm-foobar-d5ea0750'
const cacheId = await cache.saveCache(paths, key)
```

#### 恢复缓存

根据 `key` 和 `restoreKeys` 恢复基于 `paths` 提供的缓存。如果找到缓存，则函数返回缓存键，如果未找到缓存，则返回 undefined。

```js
const cache = require('@actions/cache');
const paths = [
    'node_modules',
    'packages/*/node_modules/'
]
const key = 'npm-foobar-d5ea0750'
const restoreKeys = [
    'npm-foobar-',
    'npm-'
]
const cacheKey = await cache.restoreCache(paths, key, restoreKeys)
```

##### 缓存段恢复超时

缓存以固定大小的多个段下载（现在使用 `128MB` 进行快速失败，之前的 `32 位` runner 使用 `1GB`，`64 位` runner 使用 `2GB`）。有时，段下载会卡住，导致工作流作业永远卡住并失败。`v3.0.4` 版本的 cache 包引入了缓存段下载超时。缓存段下载超时将允许中止段下载，从而允许作业以缓存未命中继续进行。

此超时的默认值为 10 分钟（从 `v3.2.1` 及更高版本开始，之前的版本中为 `v.3.0.4` 和 `v3.2.0` 之间都包括 60 分钟），可以通过指定名为 `SEGMENT_DOWNLOAD_TIMEOUT_MINS` 的 [环境变量](https://docs.github.com/en/actions/learn-github-actions/environment-variables) 并将超时值指定为分钟来进行自定义。