# 子代理（Sub-Agents）

子代理让你把常用的角色、流程或工具组合，封装为可复用的“智能体工具”，在 CLI 中像命令一样调用，或在其它代理内部作为工具被调用。

## 放置位置

支持两级来源，按优先级智能合并：
- 项目级：.codebuddy/agents/
- 个人全局：~/.codebuddy/agents/

支持子目录，子目录名会作为来源标签的一部分显示在描述中，便于区分同名代理。

## 文件命名与识别

- 每个 .md 文件即一个子代理，例如: .codebuddy/agents/test.md 会注册为名称 test 的代理
- 名称默认取文件名（去除 .md 扩展名），也可在 Frontmatter 中通过 name 指定

## Frontmatter 元数据

在 Markdown 顶部可使用 YAML Frontmatter 配置元数据：

```markdown
---
name: "my-agent"
description: "负责代码审阅与建议"
model: "gpt-4o"
tools: "bash,fs,github"
---

这里是该子代理的系统指令与工作说明……
```

字段说明：
- name：可选。覆盖默认名称（文件名）
- description：可选。简要描述。系统会自动附加来源标签 (project|user 以及子目录路径)
- model：可选。当前实现会解析但不强制使用，按产品配置可能由上层决定
- tools：可选。逗号分隔列表。当未指定时，会继承当前默认代理的工具集合。内部会自动移除 task 工具

## 指令内容

Frontmatter 之后的正文即为该子代理的“系统/工作指令”，在执行时会作为该代理的 instructions 传入。

## 作为工具使用

- 子代理会被标记为 asTool: true，可在其它工作流或代理中被当作工具调用
- 系统会自动打上标签：cli、custom，方便在列表中过滤

## 来源标签示例

- (project)：来自项目根的 .codebuddy/agents 目录
- (project:review)：来自 .codebuddy/agents/review/ 子目录
- (user)：来自 ~/.codebuddy/agents 目录
- (user:experiments:lint)：来自 ~/.codebuddy/agents/experiments/lint/ 子目录

该标签会自动拼接到 description 末尾，例如：代码审阅 (project:review)

## 编写建议

- 尽量在正文中写清楚输入、步骤、产出格式
- 如需依赖工具，请在 tools 中显式声明；若依赖默认工具，可省略
- 将共用角色/流程沉淀为独立 .md 文件，便于跨项目复用（放在用户目录）

## 常见问题

- 没有 tools 会怎样？会继承默认代理的工具集合
- 同名文件如何区分？通过 description 中的来源标签区分
- 可以嵌套目录吗？可以，目录路径将体现在来源标签中
