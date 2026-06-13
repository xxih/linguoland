# LinguoLand Agent Skill

把你在 LinguoLand 攒下的英语学习数据（生词、熟练度、阅读时长、书架、阅读进度）交给 Claude Code / Codex 等 AI Agent 查询、整理、导出。**只读，不改你的任何数据。**

## 安装

1. 拿到 skill 包（本目录：`SKILL.md` + 各能力 `.md`），放进你的 Agent skills 目录。
   - Claude Code：放到 `~/.claude/skills/linguoland/`（或项目级 `.claude/skills/linguoland/`）。

2. 在 LinguoLand App 内生成 API Key：**设置 → API Key → 生成**。明文**只显示一次**，立刻复制保存。

3. 把 key 配进环境变量：

   ```bash
   export LINGUOLAND_API_KEY=lgl_你的key
   ```

4. 对 Agent 说「看看我的 LinguoLand 阅读数据」「我这个月读了多久」「导出我的生词」即可。

## 能用来做什么

- 「我这个月读了多久、读了几本、新学多少词？」
- 「我的词频覆盖到哪了，能读懂新闻 / 畅读小说了吗？」
- 「把我所有生词导出来。」
- 「我书架上《XX》读到百分之几了？」
- 「最近我在背哪些词？」

## 接口契约

详见 `SKILL.md`：单入口 `POST https://api.linguoland.com/api/agent/gateway`，`Authorization: Bearer $LINGUOLAND_API_KEY`，body 带 `api_name`。发 `{"api_name":"/_list"}` 可列出全部可用接口。

## 安全

- key 绑定你的账号，只能读**你自己**的数据，无法读他人。
- key 可随时在 App 内吊销。
- 服务端只存 key 的哈希，明文丢了只能重新生成。
- 怀疑泄露：进 App 吊销那把 key，重新生成一把。
