# progress — 阅读进度

## `/reading/progress` — 我所有书的进度

无参数。回包 `{items: ReadingProgressDto[]}`。

## `/reading/progress/get` — 单本书进度

参数：`shelfEntryId`（必填）。回包单个 `ReadingProgressDto`。

### ReadingProgressDto 字段

| 字段 | 说明 |
|------|------|
| `shelfEntryId` | 对应书架条目 |
| `locator` | 阅读位置：EPUB 是 epub.js CFI 串；TXT 是 `<章节index>:<字符偏移>`。技术定位串，一般不直接给用户看 |
| `anchor` | 段落 hash 锚（恢复定位用），可 `null` |
| `percent` | **0..1** 的阅读百分比，书架进度条用；展示成百分比（`0.45`→「45%」），可 `null` |
| `updatedAt` | 最近阅读时间 ISO 串，展示转日期 |

> 回答「读到哪了」优先用 `percent`；想报某本书，先用 `/shelf/list` 把 shelfEntryId 对到书名。
