# 克隆或复用仓库

## 流程

1. 检查本地是否已有OpenMAIC
2. 有 → 展示路径，询问是否复用
3. 没有 → 建议克隆，询问确认
4. 克隆后单独确认安装依赖

## 推荐

- ✅ 优先复用已有仓库（如果是目标分支）
- ❌ 不要直接覆盖脏的仓库

## 命令

```bash
# 克隆
git clone https://github.com/THU-MAIC/OpenMAIC.git
cd OpenMAIC

# 安装依赖
pnpm install
```

## 确认要求

- 克隆前确认
- 安装依赖前确认
- 仓库有未提交更改时告知用户
