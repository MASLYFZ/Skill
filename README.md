# Skills 仓库

个人维护的 CodeBuddy / Agent Skills 集合。每个子目录是一个独立技能，入口文件为 `SKILL.md`（YAML frontmatter + 正文指令）。

## 快速开始

技能按**目录**分发，把需要的技能目录放到以下任一位置即可生效：

| 级别 | 路径 | 生效范围 |
| --- | --- | --- |
| 用户级 | `~/.codebuddy/skills/<skill-name>/` | 当前用户的所有项目 |
| 项目级 | `<项目根>/.codebuddy/skills/<skill-name>/` | 仅当前项目（可随 Git 共享） |

复制示例：

```powershell
# 用户级：安装单个技能
Copy-Item -Recurse .\code-reviewer $HOME\.codebuddy\skills\code-reviewer

# 用户级：安装全部技能
Get-ChildItem -Directory | Where-Object { $_.Name -ne '.backup' } |
  Copy-Item -Recurse -Destination $HOME\.codebuddy\skills\ -Force
```

也可以在 CodeBuddy 中通过「技能 → 导入技能」直接选择技能文件夹。

## 技能清单

### 代码质量与安全

| 技能 | 说明 | 典型触发场景 |
| --- | --- | --- |
| `code-reviewer` | 全面代码评审，聚焦安全漏洞、性能问题与编码规范 | 评审 PR、代码质量检查、安全审计 |
| `security-review` | 安全评审清单与模式：认证授权、用户输入、密钥、API 端点、支付与敏感功能 | 新增鉴权、处理用户输入/上传、接入密钥、写新接口 |
| `ponytail` | 「懒资深开发者」编码模式：YAGNI、优先标准库、不做未被要求的抽象 | 写代码、重构、修 bug（仅限编码类任务） |

### 前端与设计

| 技能 | 说明 | 典型触发场景 |
| --- | --- | --- |
| `frontend-design` | 前端视觉设计指导：美学方向、排版、配色与布局，避免模板化默认观感 | 新建 UI、重塑现有界面、确定视觉风格 |
| `ui-ux-pro-max` | UI/UX 设计智能库（本地可检索）：风格、产品配色、字体搭配、UX 规范、图标、图表、动效预设与技术栈 | 设计/评审/修复界面、组件、设计系统、可访问性、响应式 |
| `shadcn-vue` | shadcn-vue 组件与项目管理：添加、检索、修复、调试、组合组件 | 使用 shadcn-vue CLI、`components.json`、registry 与 preset |
| `webapp-testing` | 基于 Playwright 的本地 Web 应用测试工具包：功能验证、UI 调试、截图、浏览器日志 | 测试或调试本地 Web 应用 |

### Flutter / Dart

| 技能 | 说明 | 典型触发场景 |
| --- | --- | --- |
| `dart-flutter-patterns` | 生产级 Dart/Flutter 模式：空安全、不可变状态、Widget 架构、BLoC/Riverpod/Provider、GoRouter、Dio、Freezed、整洁架构 | 开发 Flutter 新功能、搭建项目、编写 Dart 代码 |
| `flutter-dart-code-review` | 库无关的 Flutter/Dart 评审清单：Widget 最佳实践、状态管理、Dart 惯用法、性能、可访问性、安全、整洁架构 | 评审 Flutter/Dart 代码 |

### AI 工程

| 技能 | 说明 | 典型触发场景 |
| --- | --- | --- |
| `mcp-builder` | 构建高质量 MCP（Model Context Protocol）服务器，Python（FastMCP）或 Node/TypeScript（MCP SDK） | 需要通过工具让 LLM 对接外部 API/服务 |
| `langchain-architecture` | 使用 LangChain 1.x 与 LangGraph 设计 LLM 应用：Agent、记忆、状态管理与工具集成 | 构建 AI Agent、复杂多步 LLM 工作流 |

### 元技能

| 技能 | 说明 | 典型触发场景 |
| --- | --- | --- |
| `skill-creator` | 编写与审阅 AgentSkills：创建、修复、校验、重构 `SKILL.md` 及配套资源 | 需要新建或改进一个技能时 |
| `find-skills` | 从开放的 agent skills 生态中发现并安装技能 | 「有没有能做 X 的技能」「帮我找个技能」 |

## 目录结构

```
skills/
├── <skill-name>/
│   ├── SKILL.md          # 必需：技能入口（frontmatter + 指令正文）
│   ├── references/       # 可选：按需加载的参考文档
│   ├── scripts/          # 可选：可执行脚本
│   └── assets/           # 可选：模板等静态资源
└── .backup/              # 各技能的 zip 备份快照
```

几个技能带有额外资源，例如 `ui-ux-pro-max`（本地数据 CSV/JSON 与检索脚本）、`mcp-builder`（参考文档与示例脚本）、`webapp-testing`（Playwright 辅助脚本）、`skill-creator`（初始化/校验脚本）。

## 编写新技能

最小可用的 `SKILL.md`：

```markdown
---
name: my-skill
description: 一句话说明这个技能做什么，以及什么时候应该使用它（触发条件）。
license: MIT
---

# My Skill

## 使用场景

- 场景一
- 场景二

## 工作流

1. 步骤一
2. 步骤二
```

要点：

- `name` 使用小写连字符，且与目录名保持一致。
- `description` 同时写清**能力**与**触发条件**，这是技能能否被自动匹配的关键。
- 正文保持精简：详细资料放 `references/`，按需读取，避免占用上下文。
- 可选 `allowed-tools`、`user-invocable`、`metadata` 等字段按需要进行声明。

建议先用 `skill-creator` 技能来生成或校验，再提交到本仓库。

## 备份

`.backup/` 下存放各技能的 zip 快照，用于回滚或迁移。更新技能后可重新打包覆盖：

```powershell
Compress-Archive -Path .\code-reviewer\* -DestinationPath .\.backup\code-reviewer.zip -Force
```

## 许可

各技能许可以其目录内的 `LICENSE` 或 `SKILL.md` frontmatter 中的 `license` 字段为准，来源归属见对应目录。
