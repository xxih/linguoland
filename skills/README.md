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

> 暂未收录任何 skill——待 owner 挑选要公开的资产后陆续加入。
