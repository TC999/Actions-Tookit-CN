# 包规范

为了支持 node-config 操作，我建议在四个库（tool-cache、core、io、exec）中添加以下内容，其中 tool-cache 依赖其他三个库：

### Core 规范

包含与运行器/环境交互所需的所有功能。

```ts
// 日志函数
export function debug(message: string): void
export function warning(message: string): void
export function error(message: string): void

/**
 * 为此操作和作业中的将来操作设置环境变量
 * @param name 要设置的变量的名称
 * @param val 变量的值
 */
export function exportVariable(name: string, val: string): void

/**
 * 导出变量并注册一个从日志中屏蔽的密钥
 * @param name 要设置的变量的名称
 * @param val 密钥的值
 */
export function exportSecret(name: string, val: string): void

/**
 * 将 inputPath 添加到 PATH 中
 * @param inputPath
 */
export function addPath(inputPath: string): void

/**
 * 获取输入选项的接口
 */
export interface InputOptions {
    /** 可选。输入是否为必需。如果必需且不存在，则会引发异常。默认为 false */
    required?: boolean;
}

/**
 * 获取输入的值。值也会被修剪。
 * 
 * @param name 要获取的输入的名称
 * @param options 可选。参见 InputOptions。
 * @returns 字符串
 */
export function getInput(name: string, options?: InputOptions): string | undefined

/**
 * 将操作的状态设置为中性
 * @param message 
 */
export function setNeutral(message: string): void

/**
 * 将操作的状态设置为失败
 * @param message 
 */
export function setFailed(message: string): void
```

### IO 规范

包含所有文件系统操作所需的函数（cli 场景，而非 fs 替代品）：

```ts
/**
 * cp/mv 选项的接口
 */
export interface CopyOptions {
    /** 可选。是否递归复制所有子目录。默认为 false */
    recursive?: boolean;
    /** 可选。是否覆盖目标中的现有文件。默认为 true */
    force?: boolean;
}

/**
 * 复制文件或文件夹。
 * 
 * @param source 源路径
 * @param dest 目标路径
 * @param options 可选。参见 CopyOptions。
 */
export function cp(source: string, dest: string, options?: CopyOptions): Promise<void>

/**
 * 递归强制删除路径
 * 
 * @param path 要删除的路径
 */
export function rmRF(path: string): Promise<void>

/**
 * 创建目录。创建两者之间的完整路径
 * 
 * @param p 要创建的路径
 * @returns Promise<void>
 */
export function mkdirP(p: string): Promise<void>

/**
 * 移动路径。
 *
 * @param source 源路径
 * @param dest 目标路径
 * @param options 可选。参见 CopyOptions。
 */
export function mv(source: string, dest: string, options?: CopyOptions): Promise<void>

/**
 * which 选项的接口
 */
export interface WhichOptions {
    /** 可选。是否检查工具是否存在。如果为 true，则在失败时引发异常。默认为 false */
    check?: boolean;
}

/**
 * 返回工具实际上是否被调用的路径。通过路径解析。
 * 
 * @param tool 工具的名称
 * @param options 可选。参见 WhichOptions。
 * @returns Promise<string> 工具的路径
 */
export function which(tool: string, options?: WhichOptions): Promise<string>
```

### Exec 规范

包含运行 node-config 依赖的工具（如 7-zip 和 tar）所需的所有函数：

```ts
/**
 * exec 选项的接口
 */
export interface IExecOptions

/**
 * 执行命令。
 * 输出将流式传输到实时控制台。
 * 返回带有返回代码的 promise
 * 
 * @param commandLine 要执行的命令
 * @param args 可选的附加参数
 * @param options 可选的执行选项。参见 IExecOptions
 * @returns Promise<number> 返回代码
 */
export function exec(commandLine: string, args?: string[], options?: IExecOptions): Promise<number> 
```

### Tool-Cache 规范:

包含下载和缓存 node 所需的所有函数。

```ts
/**
 * 从 URL 下载工具并将其流式传输到文件中
 *
 * @param url 要下载的工具的 URL
 * @returns 下载工具的路径
 */
export async function downloadTool(url: string): Promise<string>

/**
 * 提取 .7z 文件
 *
 * @param file .7z 文件的路径
 * @param dest 目标目录。可选。
 * @returns 目标目录的路径
 */
export async function extract7z(file: string, dest?: string): Promise<string>

/**
 * 提取 tar
 *
 * @param file tar 的路径
 * @param dest 目标目录。可选。
 * @returns 目标目录的路径
 */
export async function extractTar(file: string, destination?: string): Promise<string>

/**
 * 缓存目录并将其安装到工具缓存目录中
 *
 * @param sourceDir 要缓存到工具中的目录
 * @param tool 工具名称
 * @param version 工具的版本。semver 格式
 * @param arch 工具的体系结构。可选。默认为计算机体系结构
 */
export async function cacheDir(sourceDir: string, tool: string, version: string, arch?: string): Promise<string>

/**
 * 在本地已安装的工具缓存中查找工具的路径
 *
 * @param toolName 工具的名称
 * @param versionSpec 工具的版本
 * @param arch 可选的架构。默认为计算机的架构
 */
export function find(toolName: string, versionSpec: string, arch?: string): string
```