# 设置配置

CodeBuddy Code 使用分层配置系统，让您能够在不同级别进行个性化定制，从个人偏好到团队标准，再到项目特定需求。

## 🏗️ 配置层次结构

CodeBuddy Code 使用分层配置系统，设置按以下优先级合并（后者覆盖前者）：

```
命令行参数 (最高优先级)
├── 项目本地设置 (./settings.local.json)
├── 项目共享设置 (./settings.json)  
└── 用户设置 (~/.codebuddy/settings.json) (最低优先级)
```

### 配置文件位置

| 级别 | 文件路径 | 用途 | 版本控制 |
|------|----------|------|----------|
| **用户级** | `~/.codebuddy/settings.json` | 个人偏好设置 | 不提交 |
| **项目共享** | `./settings.json` | 团队共享配置 | 提交到 Git |
| **项目本地** | `./settings.local.json` | 个人项目设置 | 不提交 (添加到 .gitignore) |

### 配置作用域说明

- **用户级配置**: 影响所有项目的默认行为
- **项目共享配置**: 团队成员共享的项目标准
- **项目本地配置**: 个人在特定项目中的自定义设置

## ⚙️ 配置选项参考

### 完整配置示例
```json
{
  "model": "gpt-5",
  "cleanupPeriodDays": 30,
  "env": {
    "NODE_ENV": "development",
    "DEBUG": "true"
  },
  "includeCoAuthoredBy": false,
  "permissions": {
    "allow": ["Read", "Edit", "Bash(git:*)"],
    "deny": ["Bash(rm:*)", "Bash(sudo:*)"],
    "additionalDirectories": ["/tmp", "/var/log"],
    "defaultMode": "default"
  },
  "hooks": {
    "PreToolUse": "echo 'About to use tool: $TOOL_NAME'"
  },
  "enableAllProjectMcpServers": false,
  "enabledMcpjsonServers": ["filesystem", "git"],
  "disabledMcpjsonServers": ["dangerous-tool"],
  "autoCompactEnabled": true,
  "autoUpdates": true,
  "apiKeyHelper": "/usr/local/bin/get-api-key.sh"
}
```

### 配置选项详细说明

| 配置键 | 类型 | 默认值 | 描述 |
|--------|------|--------|------|
| `model` | string | - | 默认 AI 模型 (`gpt-5`, `gpt-4`) |
| `cleanupPeriodDays` | number | 30 | 本地聊天记录保留天数 |
| `includeCoAuthoredBy` | boolean | false | Git 提交是否包含 co-authored-by |
| `autoCompactEnabled` | boolean | - | 开启自动压缩功能 |
| `autoUpdates` | boolean | - | 自动更新设置 |
| `apiKeyHelper` | string | - | 获取认证密钥的脚本路径 |

### 环境变量配置

通过 `env` 对象为每个会话设置环境变量：

```json
{
  "env": {
    "NODE_ENV": "development",
    "API_URL": "https://api.example.com",
    "DEBUG": "codebuddy:*"
  }
}
```

### 权限系统

CodeBuddy Code 使用细粒度权限系统控制工具访问：

#### 权限配置选项

| 配置键 | 类型 | 描述 |
|--------|------|------|
| `allow` | string[] | 允许的工具和操作列表 |
| `deny` | string[] | 禁止的工具和操作列表 |
| `additionalDirectories` | string[] | 允许访问的额外目录 |
| `defaultMode` | string | 默认权限模式 |
| `disableBypassPermissionsMode` | string | 禁用权限绕过模式 |

#### 权限规则语法

```json
{
  "permissions": {
    "allow": [
      "Read",                           // 允许读取文件
      "Edit",                          // 允许编辑文件
      "Bash(git:*)",                   // 允许所有 git 命令
      "Bash(npm:install,npm:test)",    // 允许特定 npm 命令
      "Edit(src/**/*.js)"              // 只允许编辑 src 目录下的 JS 文件
    ],
    "deny": [
      "Bash(rm:*)",                    // 禁止删除命令
      "Bash(sudo:*)",                  // 禁止 sudo 命令
      "Edit(**/*.env)",                // 禁止编辑环境变量文件
      "Read(/etc/**)"                  // 禁止读取系统配置文件
    ],
    "additionalDirectories": ["/tmp", "/var/log"],
    "defaultMode": "default"
  }
}
```

#### 权限模式

| 模式 | 描述 |
|------|------|
| `default` | 标准权限检查 |
| `acceptEdits` | 自动接受编辑操作 |
| `plan` | 仅制定计划，不执行操作 |
| `bypassPermissions` | 绕过权限检查（谨慎使用） |

### MCP 服务器管理

控制 Model Context Protocol (MCP) 服务器的启用和禁用：

| 配置键 | 类型 | 描述 |
|--------|------|------|
| `enableAllProjectMcpServers` | boolean | 自动批准项目 .mcp.json 中的所有 MCP 服务器 |
| `enabledMcpjsonServers` | string[] | 明确启用的 MCP 服务器列表 |
| `disabledMcpjsonServers` | string[] | 明确禁用的 MCP 服务器列表 |

```json
{
  "enableAllProjectMcpServers": false,
  "enabledMcpjsonServers": ["filesystem", "git"],
  "disabledMcpjsonServers": ["experimental-tool"]
}
```



## 🌍 环境变量

CodeBuddy Code 支持通过环境变量进行配置。

### 认证相关

| 环境变量 | 描述 |
|----------|------|
| `CODEBUDDY_AUTH_TOKEN` | CodeBuddy 认证令牌 |

### 运行环境

| 环境变量 | 描述 |
|----------|------|
| `CODEBUDDY_INTERNET_ENVIRONMENT` | 网络环境配置 |
| `ENV_PRODUCT_CLI_ACCOUNT_TYPE` | CLI 账户类型 |

### 使用示例

```bash
# 设置认证令牌
export CODEBUDDY_AUTH_TOKEN="your-auth-token"

# 启动 CodeBuddy
codebuddy
```

## 🔧 配置管理命令

使用 `codebuddy config` 命令管理配置：

### 基本语法
```bash
codebuddy config [command] [options]
```

### 可用命令

| 命令 | 语法 | 描述 |
|------|------|------|
| `get` | `codebuddy config get <key>` | 获取配置值 |
| `set` | `codebuddy config set [options] <key> <value>` | 设置配置值 |
| `list` | `codebuddy config list` (别名: `ls`) | 列出所有配置 |
| `add` | `codebuddy config add <key> <values...>` | 向数组配置添加项目 |
| `remove` | `codebuddy config remove <key> [values...]` (别名: `rm`) | 移除配置或数组项 |

### 选项

| 选项 | 描述 | 适用命令 |
|------|------|----------|
| `-g, --global` | 设置全局配置 | `set` |

### 使用示例

#### 查看配置
```bash
# 列出所有配置
codebuddy config list

# 获取特定配置值
codebuddy config get model
codebuddy config get permissions
```

#### 设置配置
```bash
# 设置简单值
codebuddy config set model gpt-5

# 使用全局选项
codebuddy config set -g model gpt-4

# 设置复杂对象（使用 JSON 字符串）
codebuddy config set permissions '{"allow": ["Read", "Edit"], "deny": ["Bash(rm:*)"]}'
codebuddy config set env '{"NODE_ENV": "development", "DEBUG": "true"}'
```

#### 数组操作
```bash
# 添加环境变量
codebuddy config add env '{"DEBUG": "true"}'

# 移除配置
codebuddy config remove model
```

### 支持的配置键

根据代码实现和测试，目前通过 CLI 可操作的配置键有限：

| 配置键 | 类型 | CLI 支持 | 描述 |
|--------|------|----------|------|
| `model` | string | ✅ | AI 模型设置 |
| `permissions` | object | ✅ | 权限配置 |
| `env` | object | ✅ | 环境变量 |
| `apiKeyHelper` | string | ✅ | API 密钥助手脚本 |
| `cleanupPeriodDays` | number | ❌ | 清理周期（只读） |
| `includeCoAuthoredBy` | boolean | ❌ | Git 协作者（只读） |
| `hooks` | object | ❌ | 工具执行钩子（只读） |
| `statusLine` | object | ❌ | 状态行配置（只读） |
| MCP 相关配置 | various | ❌ | 通过 MCP 命令管理 |

> **注意**: 如果尝试操作不支持的配置键，会收到错误提示：`Cannot operate settings 'key'. Only these keys can be operate: permissions,model,env,apiKeyHelper`

### 完整示例

```bash
# 查看当前所有配置
codebuddy config list

# 设置默认模型
codebuddy config set model gpt-5

# 配置权限系统
codebuddy config set permissions '{
  "allow": ["Read", "Edit", "Bash(git:*)"],
  "deny": ["Bash(rm:*)", "Bash(sudo:*)"],
  "defaultMode": "default"
}'

# 设置环境变量
codebuddy config set env '{
  "NODE_ENV": "development",
  "DEBUG": "codebuddy:*",
  "API_URL": "https://api.example.com"
}'

# 验证设置
codebuddy config get model
codebuddy config get permissions
```

## 📋 常见配置场景

### 团队协作配置

**项目共享配置** (`./settings.json`):
```json
{
  "model": "gpt-5",
  "permissions": {
    "allow": ["Read", "Edit", "Bash(git:*)", "Bash(npm:*)"],
    "deny": ["Bash(rm:*)", "Bash(sudo:*)"]
  },
  "env": {
    "NODE_ENV": "development"
  }
}
```

**个人本地配置** (`./settings.local.json`):
```json
{
  "model": "gpt-4",
  "env": {
    "DEBUG": "myapp:*"
  }
}
```

### 安全配置

限制敏感操作和文件访问：

```json
{
  "permissions": {
    "allow": ["Read", "Edit(src/**)", "Bash(git:status,git:diff)"],
    "deny": [
      "Edit(**/*.env)",
      "Edit(**/*.key)",
      "Edit(**/*.pem)",
      "Bash(curl:*)",
      "Bash(wget:*)",
      "Read(/etc/**)",
      "Read(~/.ssh/**)"
    ],
    "defaultMode": "default"
  }
}
```

### 开发环境配置

针对不同开发阶段的配置：

```json
{
  "model": "gpt-5",
  "cleanupPeriodDays": 7,
  "permissions": {
    "allow": [
      "Read", "Edit", 
      "Bash(npm:*)", "Bash(yarn:*)",
      "Bash(git:*)", "Bash(docker:ps,docker:logs)"
    ],
    "additionalDirectories": ["/tmp", "./logs"]
  },
  "enabledMcpjsonServers": ["filesystem", "git"]
}
```


## 🚀 下一步

配置完成后，您可以：

- **[学习斜杠命令](slash-commands.md)** - 掌握所有内置功能
- **[探索 MCP 扩展](mcp.md)** - 添加自定义工具和服务
- **[查看故障排除](troubleshooting.md)** - 解决常见问题

---

*合适的配置让 CodeBuddy Code 更懂您的需求 ⚙️*
