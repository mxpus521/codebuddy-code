# 常见工作流

CodeBuddy Code 深度融入日常开发工作流，本指南展示如何在各种场景下发挥 AI 助手的最大价值。

> **注意**: 本文档中的所有命令示例都基于当前版本的实际功能。如果某个功能暂未实现，会在相应章节中明确标注。

## 📸 图片分析功能

### 支持的图片类型
- **截图**: 界面设计、错误信息、代码片段
- **架构图**: 系统设计、流程图、UML 图
- **设计稿**: UI 设计、原型图
- **文档**: 手写笔记、白板内容

### 使用方式

#### 1. 拖拽上传
```bash
codebuddy
> 分析这个界面设计
[拖拽图片到终端]
```

#### 2. 文件路径
```bash
codebuddy -p "根据这个设计稿实现 React 组件"
# 注意：通过拖拽或粘贴方式提供图片
```

#### 3. 剪贴板粘贴
```bash
# macOS
pbpaste | codebuddy -p "这个错误信息是什么意思？"

# Linux (需要 xclip)
xclip -selection clipboard -o | codebuddy -p "分析这个架构图"
```

### 实用场景

#### 🎨 UI 实现
```
> 根据这个设计稿实现一个响应式的导航栏
[上传设计图]

AI: 我看到这是一个现代风格的导航栏设计，包含：
- 左侧 Logo
- 中间导航菜单
- 右侧用户头像和通知按钮
- 移动端汉堡菜单

让我为您实现这个设计...
```

#### 🐛 错误诊断
```
> 这个错误是什么原因？
[上传错误截图]

AI: 从截图看到这是一个 TypeScript 类型错误：
"Property 'user' does not exist on type 'AuthState'"

问题分析：
1. AuthState 接口缺少 user 属性定义
2. 可能是状态管理器的类型定义不完整

解决方案：...
```

#### 📊 架构分析
```
> 帮我优化这个系统架构
[上传架构图]

AI: 这是一个微服务架构图，我注意到几个可以优化的点：

1. **单点故障风险**: API Gateway 没有高可用设计
2. **数据一致性**: 服务间直接数据库访问可能导致数据不一致
3. **监控缺失**: 缺少服务监控和链路追踪

优化建议：...
```

## 🔄 --resume 恢复功能

### 什么是 Resume？
Resume 功能让您能够：
- 恢复之前的对话上下文
- 继续未完成的任务
- 在同一设备的不同终端会话间保持工作状态

> **注意**: 会话数据仅存储在本地设备上，不支持跨设备同步。

### 基本使用
```bash
# 恢复最近的对话
codebuddy --resume

# 恢复特定的对话会话
codebuddy --resume session-id-12345

# 交互式选择会话恢复
codebuddy --resume
```

### 会话管理

#### 本地会话恢复
```bash
# 交互式选择要恢复的会话（仅显示本地会话）
codebuddy --resume

# 恢复特定会话（如果知道会话ID）
codebuddy --resume session-abc123

# 使用指定的会话ID开始新会话
codebuddy --session-id "550e8400-e29b-41d4-a716-446655440000"

# 继续最近的对话
codebuddy --continue
```

> **会话存储说明**: 
> - 会话数据存储在本地设备的用户目录中
> - 每个设备维护独立的会话历史
> - 会话ID在本地设备内唯一，不同设备可能有相同ID

### 实用场景

#### 📱 多终端开发（同一设备）
```bash
# 在终端窗口 A 中
codebuddy --session-id "mobile-app-dev-session"
> 开始开发一个 React Native 应用
[进行一些开发工作]

# 在同一设备的终端窗口 B 中
codebuddy --resume mobile-app-dev-session
> 继续之前的 React Native 开发，现在添加导航功能
```

> **限制说明**: 会话仅在本地设备存储，无法在不同设备间同步。如需跨设备协作，建议使用代码仓库和文档记录工作进度。

#### 🔄 任务切换
```bash
# 正在开发功能 A
codebuddy -p "实现用户注册功能"
[开发中...]

# 紧急 Bug 需要处理
codebuddy -p "修复登录页面的样式问题"
[修复完成]

# 回到功能 A
codebuddy --resume
> 继续用户注册功能的开发
```

#### 💡 跨设备协作建议
由于会话不支持跨设备同步，推荐以下协作方式：

```bash
# 方式1: 使用 Git 提交记录工作进度
git add . && git commit -m "WIP: 实现用户注册功能 - 完成表单验证"
git push origin feature/user-registration

# 方式2: 在项目中记录工作状态
echo "## 当前进度\n- [x] 表单验证\n- [ ] API 集成\n- [ ] 错误处理" > PROGRESS.md
git add PROGRESS.md && git commit -m "更新开发进度"

# 方式3: 使用 CodeBuddy 生成工作总结
codebuddy -p "总结当前的开发进度和下一步计划" --output-format json > work-summary.json
```

## 🔧 高级工作流

### 1. 代码审查流程

#### 自动化代码审查
```bash
# 审查单个文件
codebuddy -p "审查这个文件的代码质量" src/components/UserForm.tsx

# 审查整个提交
git diff HEAD~1 | codebuddy -p "审查这次提交的代码变更"

# 审查 Pull Request
gh pr diff 123 | codebuddy -p "分析这个 PR 的影响和风险"
```

#### 团队协作审查
```bash
# 生成审查报告（JSON格式便于后续处理）
codebuddy -p "生成详细的代码审查报告" --output-format json > report.json

# 检查编码规范
codebuddy -p "检查 src/ 目录是否遵循团队的编码规范"
```

### 2. 智能重构工作流

#### 渐进式重构
```bash
codebuddy -p "制定重构计划：将 Class 组件迁移到 Hooks"
```

AI 会提供：
- 📋 重构任务清单
- ⚡ 优先级排序
- 🔍 风险评估
- 📝 详细步骤

#### 批量重构
```bash
# 重构整个目录
codebuddy -p "将 src/components 下的所有 Class 组件改为函数组件"

# 更新 API 调用
codebuddy -p "将所有的 axios 调用改为使用 fetch API"
```

### 3. 测试驱动开发 (TDD)

#### 测试优先流程
```bash
codebuddy -p "我要实现一个购物车功能，先帮我写测试用例"
```

#### 测试覆盖率提升
```bash
# 分析测试覆盖率
npm run test:coverage | codebuddy -p "分析测试覆盖率报告，建议改进方案"

# 为未覆盖代码添加测试
codebuddy -p "为 src/utils/validation.js 添加单元测试"
```

### 4. 性能优化工作流

#### 性能分析
```bash
# 分析 Bundle 大小
npm run build:analyze | codebuddy -p "分析打包体积，提供优化建议"

# 分析运行时性能
codebuddy -p "分析这个性能分析结果"
# 注意：通过拖拽或粘贴方式提供性能分析图
```

#### 优化实施
```bash
codebuddy -p "制定 React 应用性能优化方案"
```

## 🎯 专业场景应用

### 前端开发
```bash
# 组件开发
codebuddy -p "创建一个可复用的 Table 组件，支持排序和分页"

# 状态管理
codebuddy -p "重构这个组件，使用 Zustand 替代 Redux"

# 样式优化
codebuddy -p "将这些 CSS 改为 Tailwind CSS"
```

### 后端开发
```bash
# API 设计
codebuddy -p "设计 RESTful API 接口，用于博客系统"

# 数据库优化
cat slow-query.sql | codebuddy -p "优化这个 SQL 查询的性能"

# 微服务架构
codebuddy -p "设计一个电商系统的微服务架构"
```

### DevOps
```bash
# CI/CD 配置
codebuddy -p "创建 GitHub Actions 工作流，实现自动化部署"

# Docker 容器化
codebuddy -p "为这个 Node.js 应用创建 Dockerfile"

# 监控告警
codebuddy -p "设计应用监控和告警策略"
```

## 💡 效率提升技巧

### 1. 使用别名简化命令
```bash
# 添加到 ~/.bashrc 或 ~/.zshrc
alias cb="codebuddy"
alias cbp="codebuddy -p"
alias cbr="codebuddy --resume"

# 使用
cb "快速分析这个错误"
cbp "设计数据库架构"
cbr  # 恢复上次会话
```

### 2. 配置管理
```bash
# 查看当前配置
codebuddy config list

# 设置默认模型
codebuddy config set model gpt-5

# 获取特定配置
codebuddy config get model
```

### 3. 批处理操作
```bash
# 批量文件处理
find src/ -name "*.js" | xargs -I {} codebuddy -p "为 {} 添加 JSDoc 注释"

# 批量代码检查
git diff --name-only | xargs codebuddy -p "检查这些文件的代码质量"
```

## 🚀 下一步

掌握了这些工作流后，您可以：

- **[探索 IDE 集成](ide-integrations.md)** - 在编辑器中无缝使用
- **[学习 MCP 扩展](mcp.md)** - 添加自定义工具和服务
- **[查看完整命令参考](slash-commands.md)** - 掌握所有内置功能

---

*工作流的艺术在于找到最适合您的方式 ✨*
