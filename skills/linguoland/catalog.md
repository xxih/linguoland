# catalog — 书城目录

平台书城的系统级数据（所有用户共享，与你的个人数据无关）。

## `/catalog/list` — 浏览目录

**参数（都可选）：**

| 参数 | 说明 |
|------|------|
| `kind` | 目录类型 |
| `language` | 语言（如 `en`） |
| `level` | 难度级别 |
| `featured` | `true` 只看精选 |
| `parentId` | 父目录 id（看某合集下的书） |

回包 `{items: CatalogItemMeta[]}`。

## `/catalog/get` — 单个目录项

参数：`id`（必填，目录项 id）。回包单个 `CatalogItemMeta`。

### CatalogItemMeta 关键字段

| 字段 | 说明 |
|------|------|
| `id` | 目录项 id |
| `title` / `author` | 原始标题 / 作者 |
| `titleI18n` / `authorI18n` | 多语言映射，`null`=无翻译；缺当前语言时回退 `title`/`author` |
| `language` | 语言 |
| `level` | 难度级别，可 `null` |
| `format` | 内容格式 |
| `coverUrl` | 封面 URL，可 `null` |
| `description` | 简介，可 `null` |
| `isFeatured` | 是否精选 |
| `license` / `sourceUrl` / `sourceAttribution` | 版权 / 来源信息 |
| `publishedAt` / `createdAt` / `updatedAt` | 时间，ISO 串展示转日期 |

> `storageRef` / `inlineText` / `toc` / `translationsRef` 是内容投递相关字段，展示给用户时一般不需要。
