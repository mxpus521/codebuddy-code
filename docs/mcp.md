# MCP (Model Context Protocol) 使用文档

## 概述

MCP (Model Context Protocol) 是一个开放标准，允许 CodeBuddy 与外部工具和数据源进行集成。通过 MCP，您可以扩展 CodeBuddy 的功能，连接到各种外部服务、数据库、API 等。

## 核心概念

### MCP 服务器
MCP 服务器是提供工具和资源的独立进程，CodeBuddy 通过不同的传输协议与这些服务器通信。

### 传输类型
- **STDIO**: 通过标准输入输出与本地进程通信
- **SSE**: 通过 Server-Sent Events 与远程服务通信
- **HTTP**: 通过 HTTP 流式传输与远程服务通信

### 配置作用域
- **user**: 全局用户配置，应用于所有项目
- **project**: 项目级配置，应用于特定项目
- **local**: 本地配置，仅应用于当前会话或工作区

## 配置文件

### 配置文件位置
- **user 作用域**: `~/.codebuddy.json`
- **project 作用域**: `<项目根目录>/.codebuddy.json` 
- **local 作用域**: `<项目根目录>/.mcp.json`

### 配置文件格式
```json
{
  "mcpServers": {
    "server-name": {
      "type": "stdio|sse|http",
      "command": "命令路径",
      "args": ["参数1", "参数2"],
      "env": {
        "ENV_VAR": "value"
      },
      "url": "http://example.com/mcp",
      "headers": {
        "Authorization": "Bearer token"
      },
      "description": "服务器描述"
    }
  },
  "projects": {
    "/path/to/project": {
      "mcpServers": {
        "local-server": {
          "type": "stdio",
          "command": "./local-tool"
        }
      }
    }
  }
}
```

## 命令行使用

### 添加 MCP 服务器

#### STDIO 服务器
```bash
# 添加本地可执行文件
codebuddy mcp add my-tool --command /path/to/tool --args arg1 arg2 --scope user

# 添加 Python 脚本
codebuddy mcp add python-tool --command python --args /path/to/script.py --scope project
```

#### SSE 服务器
```bash
# 添加 SSE 服务器
codebuddy mcp add sse-server --url https://example.com/mcp/sse --scope user
```

#### HTTP 服务器
```bash
# 添加 HTTP 流式服务器
codebuddy mcp add http-server --url https://example.com/mcp/http --scope project
```

### 使用 JSON 配置添加服务器
```bash
# 通过 JSON 字符串添加
codebuddy mcp add-json my-server --json '{"type":"stdio","command":"/usr/local/bin/tool","args":["--verbose"]}'

# 通过 JSON 文件添加
codebuddy mcp add-json my-server --json @config.json --scope user
```

### 管理 MCP 服务器

#### 列出所有服务器
```bash
# 列出所有作用域的服务器
codebuddy mcp list

# 列出特定作用域的服务器
codebuddy mcp list --scope user
```

#### 查看服务器详情
```bash
# 查看特定服务器信息
codebuddy mcp get my-server
```

#### 移除服务器
```bash
# 移除特定服务器
codebuddy mcp remove my-server

# 移除特定作用域的服务器
codebuddy mcp remove my-server --scope user
```

## 最佳实践

### 1. 作用域选择
- 使用 **user** 作用域存储个人工具和全局服务
- 使用 **project** 作用域存储项目特定的工具
- 使用 **local** 作用域存储临时或实验性工具

### 2. 安全考虑
- 避免在配置文件中存储敏感信息
- 使用环境变量传递认证信息
- 定期审查和更新服务器配置

### 3. 性能优化
- 合理配置服务器超时时间
- 避免同时运行过多的 STDIO 服务器
- 使用缓存机制减少重复连接

### 4. 错误处理
- 监控服务器连接状态
- 实现重连机制
- 记录和分析错误日志

## 故障排除

### 常见问题

#### 服务器连接失败
1. 检查命令路径是否正确
2. 验证参数和环境变量
3. 确认网络连接（对于远程服务器）
4. 查看服务器日志输出

#### 工具不可用
1. 确认服务器已成功连接
2. 检查工具权限设置
3. 验证工具兼容性

#### 配置不生效
1. 检查配置文件语法
2. 确认作用域优先级
3. 重启 CodeBuddy 应用

## 示例配置

### Python 工具服务器
```json
{
  "mcpServers": {
    "python-tools": {
      "type": "stdio",
      "command": "python",
      "args": ["-m", "my_mcp_server"],
      "env": {
        "PYTHONPATH": "/path/to/tools"
      },
      "description": "Python 工具集合"
    }
  }
}
```

### 远程 API 服务器
```json
{
  "mcpServers": {
    "api-server": {
      "type": "sse",
      "url": "https://api.example.com/mcp/sse",
      "headers": {
        "Authorization": "Bearer your-token",
        "X-API-Version": "v1"
      },
      "description": "远程 API 服务"
    }
  }
}
```

### Node.js 本地服务器
```json
{
  "mcpServers": {
    "node-server": {
      "type": "stdio", 
      "command": "node",
      "args": ["./mcp-server.js"],
      "env": {
        "NODE_ENV": "production"
      },
      "description": "Node.js MCP 服务器"
    }
  }
}
```

## 扩展开发

### 创建自定义 MCP 服务器

1. **选择实现语言**: Python、Node.js、Go 等
2. **实现 MCP 协议**: 使用官方 SDK 或自行实现
3. **定义工具接口**: 描述工具功能和参数
4. **处理请求**: 接收和处理来自 CodeBuddy 的请求
5. **返回结果**: 按 MCP 格式返回执行结果

### SDK 和库
- **Python**: `@ModelContextProtocol/python-sdk`
- **TypeScript/JavaScript**: `@ModelContextProtocol/typescript-sdk`
- **其他语言**: 参考官方文档实现

## 相关链接

- [MCP 官方文档](https://modelcontextprotocol.io/)
- [MCP GitHub 仓库](https://github.com/modelcontextprotocol)
- [CodeBuddy 官方文档](https://cnb.cool/codebuddy/codebuddy-code/-/blob/main/docs)
