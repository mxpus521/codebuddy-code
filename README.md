# CodeBuddy Code

<div align="center">
  <img src="codebuddy-cat.png" alt="CodeBuddy Code" width="200" height="200">
  
  <h3>腾讯云官方智能编码工具</h3>
  <p>CodeBuddy Code 是一个智能编程助手，深度集成到您的开发环境中，精准理解您的项目结构，通过自然语言交互帮助您高效编程——自动化重复任务、深度解析代码逻辑、优化开发工作流程。支持终端、IDE 无缝使用，让编程更智能、更高效。</p>

  ![](https://img.shields.io/badge/Node.js-18%2B-brightgreen?style=flat-square) [![npm]](https://www.npmjs.com/package/@tencent-ai/codebuddy-code)

[npm]: https://img.shields.io/npm/v/@tencent-ai/codebuddy-code.svg?style=flat-square


</div>

---

## 🚀 快速开始

### 安装

```bash
# 全局安装
npm install -g @tencent-ai/codebuddy-code
```

### 使用

在任意项目目录下运行：

```bash
codebuddy
```

首次使用需要进行身份认证，按照提示完成登录即可。

## ✨ 主要功能

### 🤖 智能编程助手
- **自然语言交互**：用中文或英文描述需求，AI 自动理解并执行
- **代码生成与优化**：根据描述生成高质量代码，自动优化现有代码
- **智能调试**：快速定位和修复代码问题

### 📁 项目理解
- **深度代码分析**：理解项目结构、依赖关系和业务逻辑
- **上下文感知**：基于项目历史和当前状态提供精准建议
- **多语言支持**：支持 JavaScript、TypeScript、Python、Java、Go 等主流语言

### 🛠️ 开发工具集成
- **终端集成**：直接在命令行中使用，无需切换工具
- **IDE 支持**：支持 VS Code、WebStorm 等主流编辑器
- **Git 工作流**：智能处理版本控制、分支管理、代码审查

### 🔧 任务自动化
- **代码重构**：自动重构代码结构，提升代码质量
- **测试生成**：自动生成单元测试和集成测试
- **文档生成**：自动生成 API 文档和代码注释

## 🎯 使用场景

### 1. 日常开发
```bash
codebuddy "帮我优化这个函数的性能"
codebuddy "为这个组件添加错误处理"
codebuddy "生成这个 API 的单元测试"
```

### 2. 代码审查
```bash
codebuddy "检查这次提交的代码质量"
codebuddy "分析潜在的安全问题"
codebuddy "建议代码改进方案"
```

### 3. 学习与探索
```bash
codebuddy "解释这个算法的实现原理"
codebuddy "这个设计模式在项目中如何使用"
codebuddy "推荐适合的第三方库"
```

## 📖 详细文档

### 命令参考

#### 基本用法
```bash
codebuddy [选项] [命令] [提示词]
```

#### 主要选项

| 选项 | 描述 | 示例 |
|------|------|------|
| `codebuddy` | 启动交互式会话（默认） | `codebuddy` |
| `codebuddy [提示词]` | 执行单次任务 | `codebuddy "修复这个 bug"` |
| `-p, --print` | 打印响应并退出（非交互式） | `codebuddy -p "解释这个函数"` |
| `-c, --continue` | 继续最近的对话 | `codebuddy -c` |
| `-r, --resume [会话ID]` | 恢复指定对话 | `codebuddy -r` |
| `--model <模型>` | 指定使用的模型 | `codebuddy --model sonnet` |
| `--ide` | 启动时自动连接 IDE | `codebuddy --ide` |
| `-d, --debug` | 启用调试模式 | `codebuddy -d` |
| `-h, --help` | 显示帮助信息 | `codebuddy -h` |
| `-V, --version` | 显示版本信息 | `codebuddy -V` |

#### 子命令

| 命令 | 描述 | 示例 |
|------|------|------|
| `config` | 管理配置 | `codebuddy config set -g theme dark` |
| `mcp` | 配置和管理 MCP 服务器 | `codebuddy mcp` |
| `doctor` | 检查系统健康状态 | `codebuddy doctor` |
| `update` | 检查并安装更新 | `codebuddy update` |
| `install` | 安装原生构建版本 | `codebuddy install stable` |

注意：部分子命令可能尚未支持。

## 🔒 隐私与安全

### 数据收集
我们仅收集以下数据用于改进产品：
- 使用统计（命令使用频率、功能使用情况）
- 错误日志（用于问题诊断和修复）

### 隐私保护
- **本地优先**：代码分析和处理优先在本地进行
- **数据加密**：所有网络传输数据均经过加密


## 🤝 社区与支持

### 反馈问题
- 在 [Issues 页面](https://cnb.cool/codebuddy/codebuddy-code/-/issues) 中提交详细问题报告
- 通过官方渠道联系技术支持（codebuddy@tencent.com）


## 📝 更新日志

查看 [CHANGELOG.md](CHANGELOG.md) 了解详细的版本更新记录。

---

<div align="center">
  <p>由 <a href="https://cloud.tencent.com/">腾讯云</a> 团队开发维护</p>
  <p>© 2025 Tencent Cloud. All rights reserved.</p>
</div>