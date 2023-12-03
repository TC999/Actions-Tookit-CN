# `@actions/tool-cache`

> 用于下载和缓存工具所需的函数。

## 用法

#### 下载

您可以使用此功能从下载 URL 下载工具（或其他文件）：

```js
const tc = require('@actions/tool-cache');

const node12Path = await tc.downloadTool('https://nodejs.org/dist/v12.7.0/node-v12.7.0-linux-x64.tar.gz');
```

#### 提取

然后可以以特定于平台的方式提取这些文件：

```js
const tc = require('@actions/tool-cache');

if (process.platform === 'win32') {
  const node12Path = await tc.downloadTool('https://nodejs.org/dist/v12.7.0/node-v12.7.0-win-x64.zip');
  const node12ExtractedFolder = await tc.extractZip(node12Path, 'path/to/extract/to');

  // 或者替代地
  const node12Path = await tc.downloadTool('https://nodejs.org/dist/v12.7.0/node-v12.7.0-win-x64.7z');
  const node12ExtractedFolder = await tc.extract7z(node12Path, 'path/to/extract/to');
}
else if (process.platform === 'darwin') {
  const node12Path = await tc.downloadTool('https://nodejs.org/dist/v12.7.0/node-v12.7.0.pkg');
  const node12ExtractedFolder = await tc.extractXar(node12Path, 'path/to/extract/to');
}
else {
  const node12Path = await tc.downloadTool('https://nodejs.org/dist/v12.7.0/node-v12.7.0-linux-x64.tar.gz');
  const node12ExtractedFolder = await tc.extractTar(node12Path, 'path/to/extract/to');
}
```

#### 缓存

最后，您可以将这些目录缓存在我们的工具缓存中。如果要在工具的不同版本之间切换，或者在为自托管运行时保存工具时保存工具，这将非常有用。

在此步骤中，您通常会将其添加到路径中：

```js
const tc = require('@actions/tool-cache');
const core = require('@actions/core');

const node12Path = await tc.downloadTool('https://nodejs.org/dist/v12.7.0/node-v12.7.0-linux-x64.tar.gz');
const node12ExtractedFolder = await tc.extractTar(node12Path, 'path/to/extract/to');

const cachedPath = await tc.cacheDir(node12ExtractedFolder, 'node', '12.7.0');
core.addPath(cachedPath);
```

您还可以缓存文件以供重用。

```js
const tc = require('@actions/tool-cache');

const cachedPath = await tc.cacheFile('path/to/exe', 'destFileName.exe', 'myExeName', '1.1.0');
```

#### 查找

最后，您可以找到之前缓存的目录和文件：

```js
const tc = require('@actions/tool-cache');
const core = require('@actions/core');

const nodeDirectory = tc.find('node', '12.x', 'x64');
core.addPath(nodeDirectory);
```

甚至可以找到工具的所有缓存版本：

```js
const tc = require('@actions/tool-cache');

const allNodeVersions = tc.findAllVersions('node');
console.log(`Versions of node available: ${allNodeVersions}`);
```
