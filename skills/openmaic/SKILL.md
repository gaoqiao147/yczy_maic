---
name: openmaic
description: Guided SOP for setting up and using OpenMAIC from OpenClaw. Use when the user wants to clone the OpenMAIC repo, choose a startup mode, configure recommended API keys, start the service, or generate a classroom from requirements or a PDF. Run one phase at a time and ask for confirmation before each state-changing step.
user-invocable: true
metadata: { "openclaw": { "emoji": "🏫" } }
---

# OpenMAIC Skill

Use this as a guided, confirmation-heavy SOP. Do not compress the whole setup into one reply and do not perform state-changing actions without explicit user confirmation.

## 核心原则

- **一步一步来**：每个阶段完成后，再进入下一阶段
- **先确认再行动**：任何状态变更前必须得到用户确认
- **不碰用户密钥**：不帮用户写API Key，不要求用户在聊天中粘贴Key
- **复用现有资源**：如果仓库已存在，询问是否复用而不是直接克隆

## 技能配置（可选）

如配置了以下选项，会作为默认值：

- `accessCode`：存在时默认用托管模式，直接跳过模式选择
- `repoDir`：本地模式时的默认仓库路径
- `url`：本地模式时的默认服务地址

> 即使有配置，仍然需要确认后再执行

## SOP Phases

### 0. 选择模式

检查技能配置是否有 `accessCode`：
- **有** → 直接进入托管模式，跳过阶段1-4
- **没有** → 询问用户：
  1. **托管模式**（推荐）- 需要access code，无需本地配置
  2. **本地运行** - 克隆仓库、配置Key、启动服务

### 1. 克隆或复用仓库

查看 [clone.md](references/clone.md)

用户未安装过，或需要确认使用哪个本地仓库时使用。

### 2. 选择启动模式

查看 [startup-modes.md](references/startup-modes.md)

仓库确认后，列出可用模式，推荐一个并等待用户选择。

### 3. 配置Provider Keys

查看 [provider-keys.md](references/provider-keys.md)

生成课堂前必须完成。推荐Provider路径，告诉用户具体要修改哪个配置文件。

核心LLM配置完成后，询问是否启用可选功能（Web搜索/图片/视频/TTS）。

### 4. 启动并验证

选择启动模式 + 配置Key完成后，启动服务并验证健康状态：`GET {url}/api/health`

### 5. 生成课堂

查看 [generate-flow.md](references/generate-flow.md)

仅在服务健康时使用。用户明确要求生成时，直接提交Job无需二次确认。

## 回复风格

- **简洁明确**：每个步骤短小清晰
- **给出选项**：让用户做选择时，提供2-3个具体选项，第一项是推荐的
- **说明原因**：推荐时用一句话解释为什么
- **告知下一步**：完成后说明改变了什么，下一步确认什么
- **返回URL**：课堂链接直接给原始URL，不要加粗/链接/代码格式
