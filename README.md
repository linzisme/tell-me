# tell-me

把需求、原理、变更、方案或问题，讲成一组从上到下阅读的叙事卡片。

支持在具备 HTML widget 能力的客户端内联展示；其他环境生成深色 HTML 页面。也可以按需导出 PNG 长图。输出使用提问者的语言。

## 安装

准备好 Node.js 和 Git，在你的项目目录运行：

```bash
npx skills add linzisme/tell-me --skill tell-me
```

按照提示选择要使用的 AI 工具。默认安装到当前项目；希望全局使用时加 `-g`：

```bash
npx skills add linzisme/tell-me --skill tell-me -g
```

只安装到指定工具，例如 Claude Code 或 Codex：

```bash
npx skills add linzisme/tell-me --skill tell-me -a claude-code
npx skills add linzisme/tell-me --skill tell-me -a codex
```

查看仓库可安装的技能：

```bash
npx skills add linzisme/tell-me --list
```

安装工具及其最新环境要求见 [skills 官方说明](https://github.com/vercel-labs/skills)。本仓库不需要 npm 包，也不要求配置 API key。

## 使用

在 Claude Code 中显式调用：

```text
/tell-me 用卡片讲清楚这个 PR 改了什么、为什么改、怎么验证
/tell-me 解释 OAuth 的授权流程
/tell-me 把这份需求整理成目标、范围、关键规则和验收条件
```

在 Codex 中显式调用：

```text
$tell-me 讲清楚这个方案的取舍和待确认问题
```

不带主题时，使用当前对话主题。提供文档、diff、PR 或代码时，会先读取材料再组织讲解。技能保留手动触发设置；其他客户端是否遵守该设置取决于其实现。

## 输出与环境

- **内联卡片**：需要客户端提供 HTML widget 能力。普通终端不能直接显示内联卡片。
- **HTML 页面**：无 widget 时生成独立深色页面，适配窄屏。原技能使用 `/tmp` 和 macOS 的 `open`；Linux / Windows 用户应让 agent 使用系统临时目录和对应的浏览器打开方式。远程环境可下载 HTML 后本地打开。
- **PNG 长图**：可选，需要 Python、Playwright 和 Google Chrome。技能内附导出示例；这些依赖不会随技能自动安装。

卡片按内容组织：需求关注范围和验收，原理关注步骤和边界，变更关注前后差异和验证，方案关注取舍，问题关注原因和解决路径。

## 仓库结构

```text
skills/tell-me/
├── SKILL.md
└── agents/openai.yaml
```

`SKILL.md` 是技能指令，`agents/openai.yaml` 保留 Codex 的显式调用策略。

## 许可证

采用 [PolyForm Noncommercial 1.0.0](LICENSE)。允许许可证规定的非商业使用、修改和分发；商业用途需另行获得作者授权。完整适用范围和条件以许可证原文为准。

技能目录内同时附带许可证，安装后可查看 `tell-me/LICENSE`。
