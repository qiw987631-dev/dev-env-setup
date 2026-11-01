# 环境配置笔记

> 本目录记录开发环境的安装与配置过程。

## 当前环境清单

| 组件 | 版本 | 位置 / 方式 |
|---|---|---|
| OpenCode | v2.2.5 | npm 全局安装 |
| Node.js | v22.22.2 | 系统自带 |
| Git | 2.56.0 | `C:\Program Files\Git`（winget 安装） |
| 仓库根目录 | — | `E:\AI LAB\dev-env-setup` |

## OpenCode 提供商配置要点

配置文件位于 `C:\Users\<用户名>\.config\opencode\opencode.json`，
模板见仓库 `task01-env/opencode.example.json`。

核心结构（自定义 OpenAI 兼容提供商）：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "deepseek": {
      "npm": "@ai-sdk/openai-compatible",
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

要点：

- `{env:变量名}` 让密钥走环境变量，配置文件可公开
- `provider` 下的键名（如 `deepseek`）就是 `provider/model` 里的提供商 ID
- `npm` 字段指定适配器，任何 OpenAI 兼容接口都用 `@ai-sdk/openai-compatible`
