# 代理服务器支持

自托管的运行器[可以配置](https://help.github.com/en/actions/hosting-your-own-runners/using-a-proxy-server-with-self-hosted-runners)在企业中在代理服务器后运行。

为了让操作在代理服务器后**正常工作**：

  1. 使用 [tool-cache](/packages/tool-cache) 版本 >= 1.3.1
  2. 可选使用 [actions/http-client](/packages/http-client)

如果您使用其他的HTTP客户端，请参考[由运行器设置的环境变量](https://help.github.com/en/actions/hosting-your-own-runners/using-a-proxy-server-with-self-hosted-runners)。