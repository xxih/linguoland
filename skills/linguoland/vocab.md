# vocab — 生词 / 词库

LinguoLand 的词以**词族（cluster）**为单位组织：一个词族 root（如 `run`）下挂屈折形（running/ran）与释义。熟练度三档：`unknown`（陌生）/ `learning`（学习中）/ `known`（已掌握）。

## `/vocab/stats` — 词库统计

无参数。回包：

| 字段 | 说明 |
|------|------|
| `unknown` / `learning` / `known` | 三档词数（按词族计） |
| `total` | 词族总数 |
| `recentClusters[]` | 最近接触的词族 `{clusterRoot, lastSeenAt, lookupCount}`（lastSeenAt 是时间戳，展示转日期） |

## `/vocab/sync` — 全量词库同步

无参数。返回你标记过的全部词，按词族组织。每个词族含 `clusterRoot` + `entries[]`（词条），词条含 id、词面、释义、屈折形（`inflections`）、派生路径（`morphemes`，仅派生词）。**导出生词时用这个接口取全量**（见 `export.md`）。

## `/vocab/freq-bands` — 词频分层覆盖率

无参数。按 COCA 词频阶梯统计你的掌握覆盖。回包 `bands[]`，每项：

| 字段 | 说明 |
|------|------|
| `limit` | 该频带的累计 rank 上限：`1000`=开口说话 / `3000`=日常会话 / `5000`=读懂新闻 / `10000`=畅读小说 / `20000`=母语级 |
| `total` | 该频带内的词族总数（分母） |
| `known` / `learning` | 你在该频带已掌握 / 学习中的数 |

可据此回答「我能读懂新闻了吗」——看 5000 频带 known/total 覆盖率。

## `/vocab/sources` — 词汇来源分类

无参数。回包 `{sources: string[]}`，值如 `wordbooks` / `imported` / `manual`，表示你的词来自词书 / 导入 / 手动标记。

## `/vocab/wordbook-shelf` — 词书书架

无参数。回包：

| 字段 | 说明 |
|------|------|
| `mine` | 「我的词库」`{learning, known}` |
| `primaryKey` | 当前选定的目标词书 key；未设为 `null` |
| `books[]` | 各词书进度 `{key, name, description, total, known, learning, unknown}` |

## `/vocab/search` — 在我的词库搜词

| 参数 | 必填 | 说明 |
|------|------|------|
| `query` | 是 | 搜索词，至少 2 字符，headword 前缀匹配 |
| `limit` | 否 | 返回上限，默认 20，最大 50 |

回包 `entries[]`，词条含 id / 词面 / 释义等。
