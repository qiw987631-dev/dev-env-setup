# 技术摘记

> 阅读外部资料后的要点摘录。原则：只记「我理解后用自己的话写的东西」，拒绝搬运百科。

## 1. OpenCode 的模型接入机制（来自[官方文档](https://opencode.ai/docs/providers)）

- OpenCode 基于 [AI SDK](https://ai-sdk.dev/)，通过 `models.dev` 目录预载 75+ 提供商
- 自定义提供商只需三个关键信息：适配器包名（`npm` 字段）、接口地址（`baseURL`）、密钥
- 任何「OpenAI 兼容」接口（DeepSeek、ModelScope、SiliconFlow、智谱……）都能用同一个适配器 `@ai-sdk/openai-compatible` 接入

**我的理解**：所谓「OpenAI 兼容」就是「请求/响应 JSON 格式和 OpenAI 一模一样」，所以一个适配器通吃。

## 2. 待读清单

| 资料 | 链接 | 优先级 | 状态 |
|---|---|---|---|
| 廖雪峰 Git 教程 | https://www.liaoxuefeng.com/wiki/896043488029600 | 高（Task 04） | ⬜ |
| Bash 教程（阮一峰） | https://wangdoc.com/bash/ | 高（Task 03） | ⬜ |
| Docker 官方 Get Started | https://docs.docker.com/get-started/ | 中（Task 06） | ⬜ |
| 廖雪峰 Python 教程 | https://www.liaoxuefeng.com/wiki/1016959663602400 | 高（Task 07） | ⬜ |

## 3. 摘记格式约定

```markdown
## 标题
- 来源：<链接>
- 日期：<日期>
- 要点：<用自己的话写 3~5 条>
- 疑问：<没看懂的地方，之后问组长/AI>
```
