# 快速入门指南

欢迎使用 CodeBuddy Code！这份指南将帮助您在 5 分钟内上手，体验自然语言驱动的编程助手。

## 🎯 开始之前

### 系统要求
- **Node.js**: 版本 18.0 或更高
- **操作系统**: macOS、Linux 或 Windows

### 验证环境
```bash
# 检查 Node.js 版本
node --version  # 应显示 v18.0.0 或更高

# 检查 npm 版本
npm --version
```

## ⚡ 极速安装

### 全局安装（推荐）
```bash
npm install -g @tencent-ai/codebuddy-code
```

### 验证安装
```bash
codebuddy --version
```

## 🚀 首次体验

### 1. 进入项目目录
```bash
cd /path/to/your/project
```

### 2. 启动交互模式
```bash
codebuddy
```

您将看到欢迎界面：
```
🤖 CodeBuddy Code v1.0.0
💡 输入 /help 查看可用命令
📝 开始对话，让 AI 成为您的编程伙伴

> 
```

### 3. 尝试第一个对话
```
> 帮我分析这个项目的结构
```

CodeBuddy 会自动扫描您的项目文件，并提供详细的结构分析。

## 💡 核心使用模式

### 交互式对话模式
最自然的使用方式，适合探索性开发：

```bash
codebuddy
```

**典型对话示例：**
```
> 我想给这个 React 组件添加一个加载状态
> 帮我重构这个函数，让它更易读
> 这段代码有什么潜在的性能问题？
> 为这个 API 接口写单元测试
```

### 单次命令模式
适合脚本化和自动化场景：

```bash
# 直接提问
codebuddy "优化这个 SQL 查询的性能"

# 管道输入
cat error.log | codebuddy "分析这些错误日志"

# 文件分析
codebuddy "审查 src/utils.js 的代码质量"
```

### 项目级操作
处理复杂的跨文件任务：

```bash
# 项目重构
codebuddy "将所有组件从 class 组件迁移到函数组件"

# 代码规范
codebuddy "检查整个项目的 TypeScript 类型定义"

# 测试覆盖
codebuddy "为 services 目录下的所有文件添加单元测试"
```

### 快捷键
| 快捷键 | 功能 |
|--------|------|
| `↑/↓` | 浏览命令历史 |
| `Tab` | 命令自动补全 |
| `Ctrl+C` | 退出程序 |

## 🎓 进阶学习

恭喜您完成快速入门！接下来推荐阅读：

- **[常见工作流](common-workflows.md)** - 学习扩展思考、图片分析等高级功能
- **[IDE 集成](ide-integrations.md)** - 在 VS Code、JetBrains 中使用
- **[斜杠命令](slash-commands.md)** - 掌握所有内置命令
- **[MCP 集成](mcp.md)** - 扩展自定义工具

## 💬 获取帮助

遇到问题？我们随时为您提供支持：

- 🐛 [提交 Bug](https://cnb.cool/codebuddy/codebuddy-code/-/issues)
- 📧 技术支持：codebuddy@tencent.com
- 📚 [完整文档](../README.md)
- 🌐 [官方网站](https://copilot.tencent.com/cli)

---

*现在开始，让 AI 成为您的编程伙伴! 🚀*
