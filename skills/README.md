# Skills

这里收录可公开复用的 [Claude Code](https://claude.com/claude-code) skill。

## 目录结构

每个 skill 一个子目录，目录名即 skill 名（kebab-case）：

```
skills/
  <skill-name>/
    SKILL.md          # 必需：带 frontmatter 的 skill 定义
    scripts/          # 可选：配套脚本
    references/       # 可选：补充参考资料
```

`SKILL.md` 的 frontmatter 至少包含 `name` 和 `description`：

```markdown
---
name: <skill-name>
description: <一句话说明这个 skill 干什么、什么时候触发>
---

# 标题

正文……
```

## 收录原则

只放**通用、可复用、不含内部凭据 / 私有基建细节**的 skill。和某个私有部署、服务器地址、密钥、内部数据强耦合的运维 SOP 留在私有仓库，不往这里搬。

## 已收录

### [`linguoland/`](linguoland/) — 你的英语阅读学习数据助手

LinguoLand 的对外开放能力。基于「**你的数据是你的，允许自由带走自由用**」的产品立场，通过只读 API 让 AI（Claude Code 等）查询、整理、导出你在 App 里攒下的学习资产：生词与熟练度、阅读统计、书架、阅读进度、书城目录。

- 只读，不会改动任何数据。
- 鉴权用你自己的个人 API Key（`lgl_` 开头，在「我 → 设置 → API Key」生成），从环境变量 `LINGUOLAND_API_KEY` 读取。
- 统一入口 `POST https://api.linguoland.com/api/agent/gateway`。

把整个 `linguoland/` 目录放进 Claude Code 的 skills 目录即可使用。详见目录内 [`SKILL.md`](linguoland/SKILL.md) 与 [`README.md`](linguoland/README.md)。
