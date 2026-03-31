# 启动模式

## 选项

### 1. 开发模式（推荐）

```bash
pnpm dev
```
- ✅ 反馈最快，适合首次配置和调试
- ❌ 不代表生产环境

### 2. 生产模式

```bash
pnpm build && pnpm start
```
- ✅ 更接近线上环境
- ❌ 启动较慢

### 3. Docker

```bash
docker compose up --build
```
- ✅ 环境隔离
- ❌ 较重，难快速调试

## 推荐顺序

1. `pnpm dev` ← 首选
2. `pnpm build && pnpm start`
3. `docker compose up --build`

## 健康检查

启动后验证：
```bash
curl -fsS http://localhost:3000/api/health
```
（技能配置了自定义url则用那个）

## 确认要求

- 让用户选一个模式
- 选完再执行
