# `@actions/exec`

## 用法

#### 基础

您可以使用此软件包以跨平台的方式执行工具：

```js
const exec = require('@actions/exec');

await exec.exec('node index.js');
```

#### 参数

您还可以传递参数数组：

```js
const exec = require('@actions/exec');

await exec.exec('node', ['index.js', 'foo=bar']);
```

#### 输出/选项

捕获输出或指定[其他选项](https://github.com/actions/toolkit/blob/d9347d4ab99fd507c0b9104b2cf79fb44fcc827d/packages/exec/src/interfaces.ts#L5):

```js
const exec = require('@actions/exec');

let myOutput = '';
let myError = '';

const options = {};
options.listeners = {
  stdout: (data: Buffer) => {
    myOutput += data.toString();
  },
  stderr: (data: Buffer) => {
    myError += data.toString();
  }
};
options.cwd = './lib';

await exec.exec('node', ['index.js', 'foo=bar'], options);
```

#### 在 PATH 中没有的工具

您可以为不在 PATH 中的工具指定完整路径：

```js
const exec = require('@actions/exec');

await exec.exec('"/path/to/my-tool"', ['arg1']);
```