# readdata — 阅读统计

> ⚠️ **调用前必读本文件。** 阅读时长字段单位是**毫秒**，最易因字段名误判（既不是秒也不是分钟）。

## `/readdata/overview` — 阅读统计总览

**参数：**

| 参数 | 必填 | 说明 |
|------|------|------|
| `period` | 否 | `week` / `month` / `year` / `all`，默认 `week` |
| `tz` | 否 | IANA 时区（如 `Asia/Shanghai`），默认 `UTC`。影响「今天 / 本周」的日界，建议传用户真实时区 |

**回包字段（`data`）：**

| 字段 | 说明 |
|------|------|
| `period` / `timezone` / `range` | 回显统计口径与时间范围 |
| `totalMs` | 当前 period 累计**活跃毫秒数**（毫秒！展示转小时/分钟） |
| `wordsRead` | 当前 period 累计已读词数；阅读速度 = `wordsRead / (totalMs/60000)` wpm |
| `avgDailyMs` | 当前 period 内已过日历日的日均毫秒数（毫秒） |
| `comparePct` | 与上一同长 period 的相对变化 `(cur-prev)/prev`；`null` = period=all 或上期为 0。`0.2` 展示「+20%」 |
| `stats.docsStarted` | 读过 N 本（全账户累计，**不切 period**） |
| `stats.docsFinished` | 读完 N 本（全账户累计） |
| `stats.daysRead` | 阅读 N 天（period 内当日 activeMs>0 或有查词算 1 天） |
| `stats.wordsNew` | 新学 N 词（period 内首次入库的词族数） |
| `histogram[]` | 按 period 自动分桶的时长分布，X 轴等距渲染 |
| `topBucket` | 时长最高的桶；histogram 全 0 时为 `null` |

## `/readdata/vocab-history` — 词库每日增减趋势

**参数：** `period`（多一个 `last30d` = 滚动近 30 天）、`tz`，同上。

**回包字段：**

| 字段 | 说明 |
|------|------|
| `buckets[].bucket` | 桶 key：week/month/last30d→`YYYY-MM-DD`；year→`YYYY-MM`；all→`YYYY` |
| `buckets[].added` | 该桶入库词数（口径：首次入库且当前为 LEARNING/KNOWN；hover 建的 UNKNOWN 不算） |
| `buckets[].removed` | 该桶移除词数 |
| `buckets[].net` | `added - removed`，可正可负 |
| `buckets[].cumulative` | 该桶结束时词库总量（由当前真实总量向前回推，最右端最精确） |
| `currentTotal` | 当前词库总量 = LEARNING + KNOWN |
| `totalAdded` / `totalRemoved` / `netChange` | period 内合计 |

## `/readdata/by-book` — 单本书的阅读统计

**参数：** `shelfEntryId`（必填）、`tz`。

**回包字段：** `totalMs`（该书累计毫秒）、`wordsRead`（该书累计已读词）、`recentDays[]`（最近 14 天活跃分布，`{date, ms}`，无活动日不返）。
