# shelf — 书架

## `/shelf/list` — 我的书架

无参数。回包 `{entries: ShelfEntryMeta[]}`。

## `/shelf/get` — 单个书架条目

参数：`shelfEntryId`（必填）。回包单个 `ShelfEntryMeta`。

### ShelfEntryMeta 字段

| 字段 | 说明 |
|------|------|
| `id` | 书架条目 id（后续 `/reading/*`、`/readdata/by-book`、`/book/coverage` 都用它） |
| `origin` | 来源：`CATALOG`（平台目录书）/ `IMPORT`（用户导入）/ `PASTE`/`URL`/`GENERATED` |
| `catalog` | origin=CATALOG 时的书籍详情（`CatalogItemMeta`，见 `catalog.md`），否则 `null` |
| `imported` | origin=IMPORT 时返回 `{title, author, format, sha256, sizeBytes, toc}`，否则 `null` |
| `inline` | origin=PASTE/URL/GENERATED 时返回，否则 `null` |
| `pinnedAt` | 置顶时刻 ISO 串；`null`=未置顶。置顶书排最前 |
| `sortOrder` | 手动拖拽顺序（升序）；`null`=未手动排过，回落 createdAt 倒序 |
| `createdAt` / `updatedAt` | ISO 串，展示转日期 |

> 取书名：CATALOG 书在 `catalog.title`，IMPORT 书在 `imported.title`，INLINE 在 `inline.title`。
