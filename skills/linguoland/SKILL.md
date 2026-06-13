---
name: linguoland
description: LinguoLand 英语阅读学习助手 — 查我的书架、阅读统计、生词库与熟练度、阅读进度、书城目录，并导出数据
version: 1.0.0
---

# LinguoLand — 你的英语阅读学习数据助手

通过 LinguoLand 对外只读 API，把你在 App 里攒下的学习资产（生词、熟练度、阅读时长、书架、阅读进度）交给 AI 查询、整理、导出。

> 产品立场：**这些数据是你的，允许自由带走自由用。** 本 skill 只读，不会改动你的任何数据。

## 支持的能力

| 能力 | 说明 | 用户示例 | 详细文档 |
|------|------|----------|----------|
| 书架 | 列出 / 查看书架条目 | "看看我的书架" | `shelf.md` |
| 阅读统计 | 阅读时长、天数、新学词、趋势 | "我这个月读了多久" "今年学了多少词" | `readdata.md` |
| 生词 / 词库 | 生词同步、熟练度统计、词频分层、词书进度、搜词 | "导出我的生词" "我陌生词有多少" "我能读懂新闻了吗" | `vocab.md` |
| 阅读进度 | 每本书读到哪了 | "我《XX》读到百分之几了" | `progress.md` |
| 书城目录 | 浏览平台书籍 | "书城有哪些精选书" | `catalog.md` |

调用任何接口前，先读对应文档了解参数与**字段口径**。

> 想把生词整本带走？`/vocab/sync` 已返回全量词库；标准格式导出（Anki / CSV）正在路上。

---

## 接口调用规范

### 统一入口

```
POST https://api.linguoland.com/api/agent/gateway
```

### 鉴权

- Header：`Authorization: Bearer $LINGUOLAND_API_KEY`
- `LINGUOLAND_API_KEY` 从环境变量取，格式 `lgl_xxxxxxxx`
- key 在 LinguoLand App 内「设置 → API Key」自助生成；**明文只显示一次**，请妥善保存
- 若未设置，提示用户：`export LINGUOLAND_API_KEY=<你的key>`
- key 绑定你的账号，只能读你自己的数据；服务端自动注入身份，**无需也不可传 userId**

### 请求格式

- **Method**：POST
- **Content-Type**：application/json
- **Body**：JSON，`api_name` 指定接口，其余为接口参数**平铺在顶层**，建议带 `skill_version`

```bash
curl -X POST "https://api.linguoland.com/api/agent/gateway" \
  -H "Authorization: Bearer $LINGUOLAND_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "/readdata/overview", "period": "month", "tz": "Asia/Shanghai", "skill_version": "1.0.0"}'
```

**参数平铺**（正确）：

```json
{"api_name":"/vocab/search","query":"run","limit":10}
```

**不要**把参数包进 `params` / `data` / `body` 对象里（不会被识别）：

```json
{"api_name":"/vocab/search","params":{"query":"run"}}
```

### 响应格式（统一信封）

所有请求都返回 HTTP 200 + 统一信封，靠 `errcode` 判成败：

- **成功**：`{"errcode": 0, "data": { ... }}` —— 业务数据在 `data` 里
- **失败**：`{"errcode": <非0>, "errmsg": "中文说明"}`

错误码：

| errcode | 含义 |
|---------|------|
| 0 | 成功 |
| 1001 | 未知 api_name（发 `{"api_name":"/_list"}` 看全部可用接口） |
| 1002 | 参数缺失 / 非法 |
| 1003 | 资源不存在（如 shelfEntryId 不属于你） |
| 1429 | 请求过于频繁（限流，每分钟上限） |
| 1500 | 服务内部错误 |

### 自描述

发 `{"api_name": "/_list"}` 返回所有可用 api_name + 参数定义 + 一句话说明。

---

## 通用规则

1. **能力文档预检**：调任何接口前，先读「支持的能力」表对应文档，确认参数、字段含义、**单位与计数口径**；禁止凭字段名猜含义。
2. **字段解释服从文档**：回包字段名与直觉冲突时以文档为准，不要直接翻译字段名。
3. **bookId / shelfEntryId**：用户说书名时，先 `/shelf/list` 或 `/catalog/list` 拿到 `id`，再做后续查询；对话中记住已查到的 id，不要让用户重复报。
4. **数据展示规范**：
   - **时间戳 / 日期字段**（`createdAt`、`updatedAt`、`lastSeenAt`、`range`、`bucket` 等 ISO 串或日期）：展示成 `YYYY-MM-DD`（或带时间），不要直接甩 ISO 原文 / 时间戳数字。
   - **阅读时长是毫秒**：所有 `*Ms` 字段（`totalMs` / `avgDailyMs` / `recentDays[].ms` 等）**单位是毫秒**，展示时转「X 小时 Y 分钟」或「X 分钟」，**不要当成秒或分钟直接念**。
   - **比例字段**：`comparePct` / `percent` 是 0..1 或带正负的比值，展示成百分比（如 `0.2` → 「+20%」，`percent: 0.45` → 「45%」）。
5. **结果展示**：列表用编号方便用户选择；书籍重点展示标题、作者；统计优先给结论再给明细。
6. **只读**：本 API 不提供任何写操作；用户想改数据请到 App 内操作。
