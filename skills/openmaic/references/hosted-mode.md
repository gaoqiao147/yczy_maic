# 托管模式

当用户有 open.maic.chat 的 access code 时使用。

## Access Code 配置

1. 读取技能配置中的 `accessCode`
2. 有 → 直接用，不让用户粘贴
3. 没有 → 告诉用户去 `~/.openclaw/openclaw.json` 添加：
   ```
   skills.entries.openmaic.config.accessCode = "sk-xxx"
   ```
   确认后再继续

4. 验证连接：`GET https://open.maic.chat/api/health` + `Authorization: Bearer <access-code>`
   - ✅ 成功 → 确认连接，进入生成
   - 401 → code无效，让用户去 open.maic.chat 重新生成
   - 网络错误 → 检查网络或切换本地模式

## 生成课堂

同 generate-flow.md，但：

- **Base URL**: `https://open.maic.chat`
- **每请求加认证头**: `Authorization: Bearer <access-code>`
- **课堂URL**: `https://open.maic.chat/classroom/{id}`

## 特性检测

生成前查询 `GET https://open.maic.chat/api/health`（带认证头）的 `capabilities`，自动决定是否启用可选特性

## 限额

- 每天10次生成（独立于Web UI）
- 403 `Daily quota exhausted` → 告知限额，次日0点重置

## 错误处理

| 状态码 | 含义 | 处理 |
|--------|------|------|
| 401 | Access code无效 | 让用户检查或重新生成 |
| 403 | 限额用完 | 告知每天10次，明天再来 |
| 500 | 服务器错误 | 重试或换本地模式 |
