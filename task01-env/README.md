# 现代开发环境搭建：OpenCode CLI + 大模型 API 接入

> 本文档记录本地 AI 开发环境的完整搭建过程：工具选型、安装、API 接入、验证结果与踩坑记录。

## 一、环境概览

| 项目 | 选择 | 版本 / 说明 |
|---|---|---|
| 操作系统 | Windows 11 (x64) | AMD64 |
| AI 工作台（IDE 方案 A） | WorkBuddy | 桌面客户端，内置模型网关，具备文件读写与终端执行能力 |
| CLI AI 工具 | OpenCode | v2.2.5，通过 npm 全局安装 |
| 运行时 | Node.js + npm | Node v22.22.2 / npm 10.9.7 |
| 模型提供商 1 | DeepSeek 官方 API | `https://api.deepseek.com/v1`，模型 `deepseek-flash`、`deepseek-v4-pro` |
| 模型提供商 2 | ModelScope（魔搭）API-Inference | `https://api-inference.modelscope.cn/v1`，模型 `Qwen/Qwen3.5-397B-A17B`、`ZhipuAI/GLM-5.2` 等 |
| 版本控制 | Git | 2.55.0 (Git Bash) |

## 二、CLI 工具安装

```bash
# 检查环境
node -v      # v22.22.2
npm -v       # 10.9.7

# 全局安装 OpenCode
npm install -g @opencode/cli

# 验证
opencode --version   # v2.2.5
```

npm 全局安装会生成三个启动器，对应不同终端环境：

| 文件 | 适用终端 | 说明 |
|---|---|---|
| `opencode.cmd` | CMD | 直接可执行 |
| `opencode.ps1` | PowerShell | 受 ExecutionPolicy 限制 |
| `opencode` | Git Bash / WSL | Shell 脚本 |

## 三、API 密钥申请

### 3.1 DeepSeek
1. 访问 `platform.deepseek.com` 注册并创建 API Key（形如 `sk-xxxxxxxx`）
2. 直接用 curl 验证可用性：

```bash
curl -s https://api.deepseek.com/v1/chat/completions \
  -H "Authorization: Bearer $DEEPSEEK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":20}'
```

### 3.2 ModelScope（魔搭）
1. 访问 `modelscope.cn` 注册 → 个人中心 → 访问令牌 → 新建令牌（形如 `ms-xxxxxxxx`）
2. **使用 API-Inference 前必须完成两步认证**（否则报错）：
   - 绑定阿里云账号并完成**实名认证**
   - 完成 **邮箱验证**
3. 验证：

```bash
curl -s https://api-inference.modelscope.cn/v1/chat/completions \
  -H "Authorization: Bearer $MODELSCOPE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"ZhipuAI/GLM-5.2","messages":[{"role":"user","content":"hi"}],"max_tokens":20}'
```

## 四、OpenCode 接入自定义提供商

OpenCode 通过 `@ai-sdk/openai-compatible` 适配任意 OpenAI 兼容接口。

**配置文件位置（Windows 用户级）**：`C:\Users\<用户名>\.config\opencode\opencode.json`

完整示例见本仓库 `opencode.example.json`，核心结构：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "deepseek/deepseek-flash",
  "provider": {
    "deepseek": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "DeepSeek",
      "options": {
        "baseURL": "https://api.deepseek.com/v1",
        "apiKey": "{env:DEEPSEEK_API_KEY}"
      },
      "models": {
        "deepseek-flash": { "name": "DeepSeek V4.1 Flash" }
      }
    }
  }
}
```

> **安全实践**：配置中使用 `{env:VAR}` 引用环境变量，密钥不落盘、不入库。

设置环境变量（PowerShell）：

```powershell
setx DEEPSEEK_API_KEY "sk-你的密钥"
setx MODELSCOPE_API_KEY "ms-你的令牌"
```

设置后需重开终端生效。

## 五、验证结果

```bash
opencode models
```

输出中可见自定义提供商已注册：

```
deepseek/deepseek-flash
deepseek/deepseek-v4-pro
modelscope/Qwen/Qwen3.5-397B-A17B
modelscope/ZhipuAI/GLM-5.2
modelscope/deepseek-ai/DeepSeek-V4.1-Flash
```

接口连通性验证：两家 API 均返回正常补全结果（见 `screenshots/`）。

## 六、终端交互使用

```bash
cd <你的项目目录>
opencode                    # 进入交互式 TUI
opencode -c                 # 继续上次会话
opencode run "解释这个项目"   # 单次对话模式
opencode upgrade            # 升级版本
```

进入 TUI 后直接在对话框输入需求，例如：

```
用 Python 写一个判断质数的函数，并解释思路
```

模型会返回代码与解释，至此完成「在终端中完成一次实际交互」的验证。

## 七、踩坑记录

| 问题 | 原因 | 解决方案 |
|---|---|---|
| `opencode auth login` 选 Google 提供商时连接 `oauth2.googleapis.com` 超时 | Google 服务在国内网络不可直连 | 改用国内可直连的提供商（DeepSeek / ModelScope），或通过 API Key 方式接入而非浏览器 OAuth |
| CMD / PowerShell 提示「不是内部或外部命令」 | npm 全局目录未加入系统 PATH | 将 `npm prefix -g` 输出的目录加入用户环境变量 `Path`，并重开终端 |
| PowerShell 报「无法加载 opencode.ps1，因为在此系统上禁止运行脚本」 | 默认执行策略为 `Restricted` | 执行 `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`，或改用 CMD |
| ModelScope 报 `associated Aliyun account is real-name verified` | 需绑定阿里云账号并实名认证 | 前往 `modelscope.cn/my/accountsettings` 完成认证 |
| ModelScope 报 `complete your profile via email verification` | 需完成邮箱验证 | 前往 `modelscope.cn/my/settings/account` 验证邮箱 |
| 脚本中调用 `opencode` 无任何输出 | 交互式 TUI 需要 TTY，非交互式管道环境下不渲染 | 在真实终端（CMD / PowerShell / Git Bash）中直接运行 |

## 八、交付物清单

| 文件 | 说明 |
|---|---|
| `README.md` | 本文档：环境搭建与上手说明 |
| `opencode.example.json` | OpenCode 提供商配置模板（密钥通过环境变量注入） |
| `.gitignore` | 排除含密钥的本地配置文件 |
| `screenshots/ide-workbench.png` | 方案 A：AI 工作台界面截图 |
| `screenshots/cli-interaction.png` | CLI 工具在终端成功交互截图 |
| `screenshots/api-verified.png` | API 连通性验证结果截图 |

## 九、环境搭建完成标准

- [x] CLI 工具安装成功，版本可查
- [x] 至少一个免费/低价 API 接入并验证通过
- [x] 在终端完成一次真实交互（代码解释 / 生成）
- [x] 配置与踩坑过程形成书面记录
