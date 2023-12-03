# @actions/io 发布记录

### 1.1.3
- 在所有操作系统的实现中，用 `fs.rm` 替换 `child_process.exec` 中的 `rmRF` [#1373](https://github.com/actions/toolkit/pull/1373)

### 1.1.2
- 在 `package-lock.json` 中更新 `lockfileVersion` 到 `v2` [#1020](https://github.com/actions/toolkit/pull/1020) 

### 1.1.1
- [修复了一个错误，其中我们错误地为 `rmrf` 路径进行了转义](https://github.com/actions/toolkit/pull/828)

### 1.1.0

- 添加了 `findInPath` 方法，用于在系统路径中定位所有匹配的可执行文件

### 1.0.2

- 在 `package.json` 中添加了 "types" 字段 [#221](https://github.com/actions/toolkit/pull/221)

### 1.0.0

- 初始版本