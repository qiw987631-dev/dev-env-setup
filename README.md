# AI-Lab 通识考核仓库（qiw987631-dev）

> 按《AI-Lab 通识任务文档（新进成员准入考核手册）》推进，所有任务卡成果按目录归档于此。

## 任务进度总览

| 任务 | 内容 | 目录 | 状态 |
|---|---|---|---|
| Task 01 | AI IDE + CLI 工具 + 大模型 API | [`task01-env/`](task01-env/README.md) | ✅ |
| Task 02 | Markdown 笔记系统 | [`task02-markdown/`](task02-markdown/AI-Lab通识学习计划与环境踩坑记录.md) | ✅ |
| Task 03 | Linux 终端基础 | `task03-linux/`（待建） | ⬜ |
| Task 04 | Git 与 GitHub 协作 | 全仓库提交历史 | 🔄 |
| Task 05 | Conda 环境隔离 | `task05-conda/`（待建） | ⬜ |
| Task 06 | Docker 容器化 | `task06-docker/`（待建） | ⬜ |
| Task 07~09 | Python 与算法 | `src/`（待建） | ⬜ |
| Task 10 | 综合微项目 CLI 工具 | `project/`（待建） | ⬜ |

## 笔记目录体系

```text
notes/
├── env-setup/         环境配置笔记
├── troubleshooting/   排错记录
├── code-lab/          代码实验
└── reading/           文献 / 技术摘记
```

## Task 01 交付物速览

- [`task01-env/README.md`](task01-env/README.md) —— 环境配置与上手说明（完整文档）
- [`task01-env/screenshots/`](task01-env/screenshots/) —— IDE 界面截图 + CLI 终端交互截图
- [`task01-env/opencode.example.json`](task01-env/opencode.example.json) —— 提供商配置模板（密钥走环境变量）
- 接入模型：DeepSeek（deepseek-flash / deepseek-v4-pro）、ModelScope（Qwen3.5 / GLM-5.2）

## 环境

| 组件 | 版本 |
|---|---|
| OpenCode | v2.2.5 |
| Node.js | v22.22.2 |
| Git | 2.56.0 |
| OS | Windows 11 x64 |
