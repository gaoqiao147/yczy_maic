# Provider Keys 配置

## 重要前提

- **不复用OpenClaw的Key** - OpenMAIC使用自己的服务端配置
- **不碰用户密钥** - 不帮写、不要求在聊天中粘贴
- **用户自己修改配置文件** - 告诉用户改哪个文件哪个字段

## 推荐流程

1. 推荐一个Provider方案
2. 问用户想在哪里配置：`.env.local`（推荐）或 `server-providers.yml`
3. 告诉用户具体要改什么
4. 等用户确认改好了再继续

## 推荐方案

### 1. 最简单 - Anthropic

```env
ANTHROPIC_API_KEY=sk-ant-...
```

一句话：配置最少，直接用

### 2. 性价比之选 - Google

```env
GOOGLE_API_KEY=...
DEFAULT_MODEL=google:gemini-3-flash-preview
```

一句话：速度快、成本低，**记得加`google:`前缀**，否则会默认解析为OpenAI模型

### 3. 已有Provider - 复用

```env
OPENAI_API_KEY=sk-...
DEFAULT_MODEL=openai:gpt-4o-mini
```

或

```env
DEEPSEEK_API_KEY=...
DEFAULT_MODEL=deepseek:deepseek-chat
```

## 配置方法

推荐用 `.env.local`（复制自 `.env.example`）

## 建议话术

✅ 可以说：
- "推荐通过 `.env.local` 配置，编辑完成后告诉我"
- "简单设置选Anthropic，追求性价比选Google+`DEFAULT_MODEL=google:gemini-3-flash-preview`，你想哪个？"

❌ 避免：
- "把API Key发给我"
- "把Key粘贴到这里"
- "我帮你写进去"

## 确认要求

- 先推荐一个方案
- 问用户选哪个配置文件
- 等用户确认改好了再继续

## Optional Features 可选功能

核心LLM配置完成后询问用户是否启用：

| 功能 | 环境变量 | 说明 |
|------|----------|------|
| Web搜索 | `TAVILY_API_KEY` | 实时网络研究增强大纲 |
| 图片生成 | `IMAGE_SEEDREAM_API_KEY` 或 `IMAGE_QWEN_IMAGE_API_KEY` | 为幻灯片生成图片 |
| 视频生成 | `VIDEO_SEEDANCE_API_KEY` 或 `VIDEO_KLING_API_KEY` | 生成短视频 |
| TTS语音 | `TTS_OPENAI_API_KEY` 或 `TTS_AZURE_API_KEY` | 文本转语音旁白 |

> 全部可选，不影响课堂生成基本功能
