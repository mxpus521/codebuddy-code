# CLI 参考

CodeBuddy Code 命令行工具的完整参考手册，基于真实命令结构提供准确的使用说明。

> 注意：部分命令为预留占位，当前版本尚未完全实现，调用时会提示不支持。请以 `--help` 实际输出为准。

## 🚀 基本语法

```bash
codebuddy [options] [command] [prompt]
```

CodeBuddy Code 默认启动交互式会话，使用 `-p/--print` 进行非交互式输出。

## 📋 全局选项

### 基础选项
```bash
-V, --version                         输出版本号
-h, --help                            显示帮助信息
-d, --debug                           启用调试模式 (默认: false)
    --verbose                         覆盖配置中的详细模式设置 (默认: false)
-p, --print                           打印响应并退出 (适用于管道) (默认: false)
```

### 输入输出格式  
```bash
--output-format <format>              输出格式 (仅与 --print 配合使用):
                                      "text" (默认), "json" (单个结果),
                                      或 "stream-json" (实时流式)
--input-format <format>               输入格式 (仅与 --print 配合使用):
                                      "text" (默认), 或 "stream-json"
                                      (实时流式输入)
```

### 会话管理
```bash
-c, --continue                        继续最近的对话 (默认: false)
-r, --resume [sessionId]              恢复对话 - 提供会话ID或交互式选择
    --session-id <uuid>               使用特定的会话ID (必须是有效的UUID)
```

### AI 模型选项
```bash
--model <model>                       当前会话的模型。提供模型名称
                                      (如 'gpt-5' 或 'gpt-4')
--fallback-model <model>              当默认模型过载时自动回退到指定模型
                                      (仅与 --print 配合使用)
```

### 安全和权限
```bash
--dangerously-skip-permissions        绕过所有权限检查。仅推荐用于无网络访问的沙箱
--permission-mode <mode>              权限模式: "acceptEdits", "bypassPermissions", 
                                      "default", "plan"
                                      支持会话级动态管理，UI实时显示当前状态
--allowedTools <tools...>             允许的工具列表 (如 "Bash(git:*) Edit")
--disallowedTools <tools...>          禁止的工具列表 (如 "Bash(git:*) Edit")
--add-dir <directories...>            允许工具访问的额外目录
```

### MCP 集成
```bash
--mcp-config <fileOrString>           从JSON文件或字符串加载MCP服务器
--strict-mcp-config                   仅使用 --mcp-config 中的MCP服务器，
                                      忽略其他MCP配置 (默认: false)
```

### IDE 集成
```bash
--ide                                 如果有且仅有一个有效IDE可用，
                                      启动时自动连接 (默认: false)
```

## 🎯 主要命令

### 交互模式
```bash
# 启动交互式对话
codebuddy

# 指定模型启动
codebuddy --model gpt-5

# 继续最近的对话
codebuddy -c

# 恢复特定会话
codebuddy -r session-id-12345

# 自动连接IDE
codebuddy --ide
```

### 单次执行模式
```bash
# 基本提问
codebuddy -p "解释这个函数的作用"

# JSON 格式输出
codebuddy -p "分析代码结构" --output-format json

# 流式输出
codebuddy -p "生成大量代码" --output-format stream-json

# 管道输入
cat error.log | codebuddy -p "分析这些错误日志"
```

### 模型和回退
```bash
# 使用特定模型
codebuddy --model gpt-5 -p "复杂的代码分析任务"

# 设置回退模型
codebuddy --model gpt-5 --fallback-model gpt-4 -p "查询"
```

### 配置命令 (config)

```bash
# 列出配置（支持）
codebuddy config list

# 获取配置（支持）
codebuddy config get model

# 设置配置（支持，限 keys: permissions, model, env, apiKeyHelper）
codebuddy config set model gpt-5
```

### MCP 命令 (mcp)

```bash
# 列出 MCP（支持）
codebuddy mcp list

# 其他子命令（帮助可见，但可能因配置缺失而不可用）
codebuddy mcp add <name> <commandOrUrl> [args...]
codebuddy mcp remove <name>
codebuddy mcp get <name>
codebuddy mcp add-json <name> <json>
```

## 🛠️ 实用命令

### 安装和更新
```bash
# 安装（当前不支持，执行会提示不支持）
codebuddy install [options] [target]

# 检查更新（支持）
codebuddy update

# 从全局npm安装迁移（当前不支持，执行会提示不支持）
codebuddy migrate-installer

# 健康检查（当前不支持，执行会提示不支持）
codebuddy doctor
```

## 🎨 输出格式详解

### text 格式 (默认)
```bash
codebuddy -p "Hello" --output-format text
# 输出: 纯文本响应
```

### json 格式
```bash
codebuddy -p "分析代码" --output-format json
# 输出: {"response": "...", "metadata": {...}}
```

### stream-json 格式
```bash
codebuddy -p "生成长代码" --output-format stream-json
# 输出: 实时流式JSON响应
```

## 🔒 安全和权限控制

### 工具权限控制
```bash
# 只允许特定工具
codebuddy --allowedTools "Read Edit" -p "修改文件"

# 禁止特定工具
codebuddy --disallowedTools "Bash" -p "分析代码"

# 允许特定Git操作
codebuddy --allowedTools "Bash(git:status,git:diff)" -p "检查Git状态"

# 跳过权限检查 (谨慎使用)
codebuddy --dangerously-skip-permissions -p "执行操作"
```

### 目录访问控制
```bash
# 添加允许访问的目录
codebuddy --add-dir /path/to/project --add-dir /tmp -p "处理文件"
```

## 📝 实用示例

### 日常开发
```bash
# 代码审查 (管道输入)
git diff | codebuddy -p "审查这些代码变更"

# 生成提交信息
git diff --cached | codebuddy -p "生成提交信息" --output-format text

# 错误日志分析
tail -f error.log | codebuddy -p "实时分析错误" --input-format stream-json
```

### 项目分析
```bash
# 项目结构分析
codebuddy -p "分析项目结构并提供改进建议" --output-format json

# 依赖分析
cat package.json | codebuddy -p "分析依赖并建议优化"
```

### 会话管理
```bash
# 开始一个有特定ID的会话
codebuddy --session-id "550e8400-e29b-41d4-a716-446655440000"

# 继续上次对话
codebuddy -c

# 恢复特定会话 (交互式选择)
codebuddy -r
```

### MCP 工作流
```bash
# 配置文件系统MCP服务器
codebuddy mcp add filesystem npx -y @modelcontextprotocol/server-filesystem ./src

# 使用MCP配置启动
codebuddy --mcp-config mcp-servers.json

# 严格MCP模式 (仅使用指定配置)
codebuddy --strict-mcp-config --mcp-config servers.json
```

## ⚡ 性能优化技巧

### 模型选择策略
```bash
# 简单任务使用快速模型
codebuddy --model gpt-4 -p "简单问题"

# 复杂任务使用高级模型
codebuddy --model gpt-5 -p "复杂分析任务"

# 设置回退模型防止过载
codebuddy --model gpt-5 --fallback-model gpt-4 -p "查询"
```

### 输出优化
```bash
# 流式输出获得即时反馈
codebuddy -p "生成大量代码" --output-format stream-json

# JSON格式便于程序处理
codebuddy -p "分析数据" --output-format json | jq '.response'
```

## 🚨 故障排除

### 调试选项
```bash
# 启用调试模式
codebuddy --debug -p "测试查询"

# 详细输出
codebuddy --verbose -p "需要详细信息的查询"

# 组合调试选项
codebuddy --debug --verbose -p "完整调试信息"
```

### 常见问题解决
```bash
# 权限问题
codebuddy --allowedTools "Read Edit Bash" -p "需要多种工具的操作"

# 会话问题
codebuddy --session-id "new-uuid" -p "开始新会话"

# MCP连接问题
codebuddy mcp list  # 检查MCP服务器状态
codebuddy --strict-mcp-config --mcp-config config.json  # 使用特定配置
```

## 🔄 命令组合和管道

### 管道操作
```bash
# 多阶段处理
find . -name "*.js" | head -5 | xargs cat | codebuddy -p "分析代码模式"

# 结合Git操作
git log --oneline -10 | codebuddy -p "分析提交历史"

# 实时日志分析
tail -f app.log | codebuddy -p "监控并分析日志" --input-format stream-json
```

### 批量操作
```bash
# 批量文件处理
for file in *.js; do
  codebuddy -p "为文件添加注释: $file" --output-format json >> results.json
done
```

## 🚀 下一步

掌握 CLI 命令后，您可以：

- **[学习交互模式](interactive-mode.md)** - 掌握键盘快捷键和技巧
- **[探索斜杠命令](slash-commands.md)** - 了解内置命令
- **[学习 MCP 集成](mcp.md)** - 扩展工具能力

---

*精确的命令行操作是高效开发的基础 ⚡*
