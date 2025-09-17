# CodeBuddy Code 斜杠命令 (Slash Commands)

CodeBuddy Code 支持斜杠命令,允许您在聊天中执行特殊操作、管理会话以及自定义常用工作流。

## 内置斜杠命令 (Built-in Slash Commands)

这些命令用于管理您的 CodeBuddy Code 会话。以下是当前的支持情况:

| 命令 | CodeBuddy 支持情况 | 描述 |
| --- |:---:| --- |
| `/help` | ✅ 支持 | 显示帮助信息,并提供反馈渠道的指引。 |
| `/clear` | ✅ 支持 | 清除当前对话的上下文历史记录,开始一个新的对话。 |
| `/login` | ✅ 支持 | 登录到您的云开发环境。 |
| `/logout` | ✅ 支持 | 退出当前的云开发环境。 |
| `/doctor` | ✅ 支持 | 检查 CodeBuddy Code 的状态和环境。 |
| `/status` | ✅ 支持 | 显示当前仓库和会话的状态。 |
| `/add-dir` | ✅ 支持 | 提示时添加目录内容。 |
| `/agents` | ❌ 暂不支持 | 管理实验性 AI 智能体 |
| `/compact`| ✅ 支持 | 压缩上下文。 |
| `/config`| ✅ 支持 | 查看或修改本地配置。 |
| `/cost` | ✅ 支持 | 显示会话的成本和 Token 使用情况。 |
| `/init` | ✅ 支持 | 初始化一个新的 CodeBuddy 存储库。 |
| `/mcp` | ✅ 支持 | 管理 MCP 连接。 |
| `/memory`| ❌ 暂不支持 | 管理长期记忆 |
| `/model` | ✅ 支持 | 切换或查看当前使用的 AI 模型。 |
| `/upgrade` | ✅ 支持 | 在浏览器中打开升级页面，查看高级功能和订阅选项。 |

---

## 自定义斜杠命令 (Custom Slash Commands)

这是 CodeBuddy Code 最强大的功能之一。您可以将常用的提示(Prompts)、脚本和工作流封装成可复用的自定义命令,从而极大地提升效率。

### 创建自定义命令

自定义命令通过在特定目录中创建 `.md` (Markdown) 文件来定义。

1.  **项目级命令**: 在您的项目根目录下创建 `.codebuddy/commands/` 文件夹。此处的命令对所有项目协作者可用。
2.  **个人全局命令**: 在您的用户主目录下创建 `~/.codebuddy/commands/` 文件夹。此处的命令在您所有的项目中都可用。

创建一个命令,只需在上述任一目录中添加一个 `.md` 文件即可。例如,`test.md` 文件会自动注册为 `/test` 命令。

### Frontmatter 与元数据

您可以在 Markdown 文件的顶部使用 YAML Frontmatter 来定义命令的元数据。

```markdown
---
description: "为我的项目运行单元测试并报告结果。"
argument-hint: "[test-file]"
---

请为我运行 `npm run test -- [test-file]` 命令,并总结测试结果。如果未提供测试文件,则运行所有测试。
```

支持的元数据字段:
*   `description`: 命令的简短描述,会在自动补全提示中显示。
*   `argument-hint`: 描述命令需要的参数,为用户提供输入提示。

### 使用参数

您的自定义命令可以接收参数,就像 Shell 脚本一样。

*   `$1`, `$2`, `$3`, ...: 按位置访问单个参数。

**示例: `greet.md`**
```markdown
---
description: "发送一个可定制的问候。"
argument-hint: "[name]"
---

向 **$1** 说"你好！"。如果 $1 为空,则向"世界"问好。
```
*   调用 `/greet "CodeBuddy"` 时,`$1` 的值就是 `"CodeBuddy"`。

### 执行 Shell 命令

在命令的任何一行前面加上 `!` 前缀，命令用反引号包围，该行就会被当作 Shell 命令执行，其输出 (`stdout`) 会被捕获并注入到上下文中，供 AI 后续分析。

**示例: `status.md`**
```markdown
---
description: "显示当前的 git 仓库状态并进行分析。"
---

!`git status`

请基于上面的 `git status` 输出,为我总结当前分支的状况。
```
