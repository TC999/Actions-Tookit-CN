# `@actions/io`

> 用于命令行文件系统场景的核心功能

## 用法

#### mkdir -p

递归创建目录。遵循 [man mkdir](https://linux.die.net/man/1/mkdir) 中指定的规则，使用 `-p` 选项：

```js
const io = require('@actions/io');

await io.mkdirP('path/to/make');
```

#### cp/mv

复制或移动文件或文件夹。遵循 [man cp](https://linux.die.net/man/1/cp) 和 [man mv](https://linux.die.net/man/1/mv) 中指定的规则：

```js
const io = require('@actions/io');

// 对于目录，必须设置递归为 true
const options = { recursive: true, force: false }

await io.cp('path/to/directory', 'path/to/dest', options);
await io.mv('path/to/file', 'path/to/dest');
```

#### rm -rf

递归删除文件或文件夹。遵循 [man rm](https://linux.die.net/man/1/rm) 中指定的规则，使用 `-r` 和 `-f` 规则：

```js
const io = require('@actions/io');

await io.rmRF('path/to/directory');
await io.rmRF('path/to/file');
```

#### which

获取工具的路径并通过路径解析。遵循 [man which](https://linux.die.net/man/1/which) 中指定的规则。

```js
const exec = require('@actions/exec');
const io = require('@actions/io');

const pythonPath: string = await io.which('python', true)

await exec.exec(`"${pythonPath}"`, ['main.py']);
```