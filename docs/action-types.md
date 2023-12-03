# 操作类型

有两种类型的操作：JavaScript 操作和 Docker 操作。

- **JavaScript 操作**：JavaScript 操作在主机机器上运行。工作单元与环境解耦。
- **Docker 操作**：容器操作是一个容器，它既携带工作单元，又携带环境及其依赖项打包成一个容器。

两者都可以访问工作空间、GitHub 事件载荷和上下文。

## 为什么我会选择 Docker 操作？

Docker 操作既携带工作单元，又携带环境。

这创建了一个更一致和可靠的工作单元，使用该操作的消费者无需担心工具集及其依赖关系。

Docker 操作目前仅限于 Linux。

## 为什么我会选择主机操作？

JavaScript 操作将工作单元从环境解耦，并直接在主机机器或虚拟机上运行。

考虑一个简单的示例，测试一个 Node 库在 Node 8、10 上的情况，并运行自定义操作。每个作业都会在主机上设置一个 Node 版本，自定义操作将在每个环境上运行其工作单元（node8+ubuntu16、node8+windows-2019 等）。

```yaml
on: push

jobs:
  build:
    strategy: 
      matrix:
        node: [8.x, 10.x]
        os: [ubuntu-16.04, windows-2019]
    runs-on: ${{matrix.os}}
    actions:
    - uses: actions/setup-node@v3
      with:
        version: ${{matrix.node}}
    - run: | 
        npm install
    - run: |
        npm test
    - uses: actions/custom-action@v1
```

JavaScript 操作可以在支持主机操作运行时的任何环境上运行，目前支持的是 Node 12。但是，运行工具集的主机操作期望运行环境在其 PATH 中具有该工具集，或者使用 setup-* 操作在需要时获取它。