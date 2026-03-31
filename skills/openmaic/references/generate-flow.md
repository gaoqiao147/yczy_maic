# 课堂生成流程

## 前置条件

- ✅ 仓库路径已确认
- ✅ 启动模式已选择
- ✅ 服务健康（`GET {url}/api/health` 返回正常）
- ✅ Provider Keys 已配置

> **托管模式**：使用 open.maic.chat 时，以上条件自动满足，所有请求需加 `Authorization: Bearer <access-code>` 头

## 仅文本生成

用户明确要求生成 + 前置条件满足 → 直接提交，无需二次确认

```text
POST {url}/api/generate-classroom
```

```json
{
  "requirement": "创建一个高中量子力学入门课堂"
}
```

### 支持的字段

- `requirement` (required)
- optional `pdfContent`
- optional `language` (`"zh-CN"` | `"en-US"`, defaults to `"zh-CN"`) — any other value silently falls back to `"zh-CN"`
- optional `enableWebSearch` (boolean) — include web search context in outline generation
- optional `enableImageGeneration` (boolean) — allow image generation metadata in outlines
- optional `enableVideoGeneration` (boolean) — allow video generation metadata in outlines
- optional `enableTTS` (boolean) — enable server-side TTS audio generation for speech actions
- optional `agentMode` (`"default"` | `"generate"`) — controls agent profile strategy:
  - `"default"` (or omitted): uses built-in default agents
  - `"generate"`: uses LLM to generate custom agent profiles tailored to the course content

### 特性检测

发送可选特性前，先查 `GET {url}/api/health` 的 `capabilities` 字段：

```json
{
  "capabilities": {
    "webSearch": true,
    "imageGeneration": false,
    "videoGeneration": false,
    "tts": true
  }
}
```

只有当 `capabilities` 为 `true` 时才发送对应的 `enableXxx: true`

## 响应处理

```json
{
  "success": true,
  "jobId": "abc123",
  "status": "queued",
  "pollUrl": "http://localhost:3000/api/generate-classroom/abc123",
  "pollIntervalMs": 5000
}
```

保存 `jobId`、`pollUrl`、`pollIntervalMs`，进入轮询

## PDF生成

1. 确认后再读取本地PDF
2. 先解析PDF：`POST {url}/api/parse-pdf`
3. 带 `requirement` + `pdfContent` 提交：`POST {url}/api/generate-classroom`

## 轮询流程

1. 每60秒轮询一次（不要频繁刷）
2. `queued` / `running` = 进行中，继续等
3. `succeeded` → 返回课堂ID和URL
4. `failed` → 返回错误信息，提示用户检查配置

### 可靠性规则

- ❌ 不要因为一次轮询失败就重启Job
- ❌ 不要尝试用请求参数修复认证/Provider错误
- ✅ 单次Agent轮询最多轮询约10分钟
- ✅ Job还在运行时结束轮询，告知用户Job ID让其稍后继续

### 超时处理

> "课堂生成仍在后台运行，Job ID: abc123，稍后可以回来继续查询进度。"

## 返回格式

成功时：
```
Classroom ID: Uyh82Y32ZK
Classroom URL:
http://localhost:3001/classroom/Uyh82Y32ZK
```

❌ 不要加粗/链接/代码格式，直接给原始URL

失败时：返回Job ID + 服务器错误，如果涉及Provider/Model问题，提示用户检查 `.env.local`