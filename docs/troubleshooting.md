
# Troubleshooting

## 系统要求

### Node.js 版本要求

CodeBuddy 需要 Node.js v18.20.8 或更高版本。

**检查版本**:
```bash
node -v
```

**升级 Node.js**: https://nodejs.org/en/download/

## Windows 平台特殊要求

### Git Bash 依赖

Windows 平台必须安装 Git Bash 才能运行 CodeBuddy。

**安装 Git Bash**:
- 下载地址: https://git-scm.com/downloads/win
- 安装时确保勾选 "Git Bash" 组件

**检测逻辑**:
1. 优先检查环境变量 `CODEBUDDY_CODE_GIT_BASH_PATH`
2. 尝试在 PATH 中查找 `git` 命令
3. 检查常见安装位置:
   - `C:/Program Files/Git/bin/bash.exe`
   - `C:/Program Files (x86)/Git/bin/bash.exe`

**自定义 Git Bash 路径**:

如果 Git Bash 安装在非标准位置,设置环境变量:
```bash
set CODEBUDDY_CODE_GIT_BASH_PATH=C:\Program Files\Git\bin\bash.exe
```

或在 PowerShell 中:
```powershell
$env:CODEBUDDY_CODE_GIT_BASH_PATH="C:\Program Files\Git\bin\bash.exe"
```

## 搜索工具问题

### Ripgrep (rg) 未找到

系统为 ripgrep 的可用性提供了多种降级机制:

1. **内置二进制文件**: vendor 目录中包含预编译的 ripgrep 二进制文件
2. **系统 PATH**: 检查系统 PATH 中是否存在 `rg` 命令
3. **自动下载**: 根据平台需要自动下载相应的二进制文件
- 二进制文件会被缓存以避免重复下载
- 支持 macOS (Intel/Apple Silicon)、Windows 和 Linux 平台

### 搜索性能优化

为获得最佳搜索性能:
- 建议通过包管理器在系统级别安装 ripgrep
- 降级下载机制虽然能提供功能,但初始设置可能较慢

## Windows 安装常见问题

### "codebuddy 不是内部或外部命令" 错误

**症状**:
- 执行 `npm install -g @tencent-ai/codebuddy-code` 安装成功
- 运行 `codebuddy` 命令时提示 "'codebuddy' 不是内部或外部命令,也不是可运行的程序或批处理文件"

**原因**: 
npm 全局安装目录未加入系统环境变量 PATH

**解决方案**:

1. 查找 npm 全局安装路径:
   ```bash
   npm config get prefix
   ```

2. 将以下路径添加到系统环境变量 PATH:
   - `%USERPROFILE%\AppData\Roaming\npm` (默认路径)
   - 或上一步命令显示的路径

3. 重启命令行窗口使环境变量生效

4. 验证安装:
   ```bash
   codebuddy --version
   ```

### Git Bash 未找到错误

**症状**:
启动 CodeBuddy 时提示需要安装 git-bash

**解决方案**:
参考上方"Windows 平台特殊要求 > Git Bash 依赖"章节

## 模型切换

### 如何切换 AI 模型

CodeBuddy 支持切换不同的 AI 模型以适应不同的使用场景和需求。

**切换方式**:

1. **使用斜杠命令** (推荐):
   ```
   /model
   ```
   - 会显示一个交互式选择界面
   - 列出所有可用的模型及其描述
   - 当前使用的模型会标记为绿色 ✓
   - 使用方向键选择,按 Enter 确认,按 Esc 退出

2. **指定具体模型名称**:
   ```
   /model [模型名称]
   ```
   - 直接切换到指定的模型
   - 适用于自定义模型名称的场景

**效果范围**:
- 切换后的模型会立即应用到当前对话
- 设置会持久保存,影响未来的所有会话

**查看当前模型**:
使用 `/status` 命令可以查看当前会话使用的模型信息

## 更新 CodeBuddy Code

### 自动更新

CodeBuddy Code 默认开启了自动更新功能:

- 使用过程中会自动检查是否有新版本
- 如果有更新,会在后台自动下载
- 下载完成后,重启命令行即可生效
- 可通过 `/config` 命令关闭或开启自动更新

### 手动更新

**方法一: 使用内置命令** (推荐):
```bash
codebuddy update
```

**方法二: 使用 npm 更新**:
```bash
npm install -g @tencent-ai/codebuddy-code@latest
```

**方法三: 先卸载再安装**:
```bash
npm uninstall -g @tencent-ai/codebuddy-code
npm install -g @tencent-ai/codebuddy-code
```

### 检查版本

**查看当前版本**:
```bash
codebuddy --version
```

**查看 npm 上的最新版本**:
```bash
npm view @tencent-ai/codebuddy-code version
```

### 更新后验证

更新完成后,建议重启终端并验证:
```bash
codebuddy --version
```
