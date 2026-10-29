# KickScope Agent Skill

Skill **0.17.1** · Chrome extension **0.17.0+**.

Connect a local Agent to the KickScope Chrome extension through MCP, then read Kickstarter Stories and image text for research. The Chrome extension is maintained and distributed separately.

## 使用

1. 在 Mac 的 Google Chrome 中安装并启用 KickScope，打开「连接 Agent」。
2. 复制页面里的 Skill 接入指令，发给你的本地 Agent。指令包含这个仓库和实际扩展 ID。
3. Agent 读取 [kickscope-research Skill](skills/kickscope-research/SKILL.md)，检查并运行配套安装器，按自身客户端配置 MCP，再实际验证连接。

也可以把这个仓库链接交给 Agent：请它安装并使用其中的 kickscope-research Skill，并提供实际 Chrome 扩展 ID。

需要 macOS、Google Chrome、Node.js 18+，以及能访问这台 Mac、支持本地 MCP stdio 的 Agent。Claude Code、Codex 等客户端各自配置；云电脑安装暂不支持。

## Features

Read public Kickstarter Story text and all or selected image OCR with Chrome AI. Query progress, stop/resume jobs and retrieve complete paginated Markdown/JSON. OCR runs in Chrome; results return to your Agent for analysis and may need manual review. A connected bridge does not prove the image model is ready.

## Versioned installation

The Skill has its own version in `skills/kickscope-research/skill.json` and SKILL.md metadata. Minimum compatible Chrome extension version is recorded separately. See the Skill CHANGELOG for release history.

Use the tag supplied by the extension's setup request. Read `skills/kickscope-research/SKILL.md` and the relevant setup reference. `release.json` lists the SHA-256 of each distributed file; verify the installer before running it. The self-contained `scripts/install.cjs` requires the actual extension ID and defaults to client-neutral mode; no npm package or extension-source checkout is needed.

Installing the Skill alone does not install the Chrome extension or establish MCP connectivity. Generic setup installs local components and emits MCP JSON, while the Agent registers it in its current client and verifies an actual `kickscope_status` call. Client-specific settings must preserve other servers and permissions.

## Scope

This public repository contains only the Skill, its local companion installer and version metadata. The installer embeds the narrow local MCP bridge implementation required for its five tools. It includes no Chrome extension source, research outputs, user profiles or credentials. No cloud AI API key is needed for local Chrome recognition. Third-party Agent clients have not all been individually tested.
