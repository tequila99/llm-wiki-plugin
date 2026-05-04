Run a health check on the wiki knowledge base and fix structural issues.

## Phase 1 — Deterministic auto-fixes (apply silently)

### 1.1 Index consistency

Read `wiki/index.md` and list all `[[slug]]` entries.
Read all files in `wiki/*.md` (excluding index.md and log.md).

- **Stale index entries**: rows in index.md whose `[[slug]]` has no corresponding `wiki/slug.md` file → remove those rows
- **Missing index entries**: `wiki/*.md` files not listed in index.md → add them (read the file to extract title, tags, first sentence as summary)

### 1.2 Missing frontmatter fields

For each `wiki/*.md` file (excluding index.md and log.md):
- If `updated:` is missing → add with today's date
- If `tags:` is missing → add `tags: []`
- If `title:` is missing → add using the H1 header or filename

### 1.3 Broken wikilinks — index

Check each `[[slug]]` in index.md. If `wiki/slug.md` doesn't exist, remove the row.

## Phase 2 — Heuristic checks (report only, do not auto-fix)

### 2.1 Orphan pages

For each `wiki/*.md` (excluding index.md, log.md):
- Count incoming `[[slug]]` references from all other wiki pages
- If count = 0: mark as orphan

### 2.2 Broken wikilinks in page bodies

Scan all `wiki/*.md` for `[[...]]` patterns.
For each found `[[slug]]`, check if `wiki/slug.md` exists.
Collect all broken links with their source file.

### 2.3 Unprocessed raw files

List all `.md` files in `raw/` recursively.
Check each filename against entries in `wiki/log.md`.
Files not mentioned in any log entry = unprocessed sources.

### 2.4 Stale pages

Find pages where `updated:` is more than 180 days ago.
These may contain outdated information.

## Phase 3 — Output report

```
## Отчёт wiki-lint — YYYY-MM-DD

### ИСПРАВЛЕНО АВТОМАТИЧЕСКИ
- [x] Удалено N устаревших записей в index.md: [[slug1]], [[slug2]]
- [x] Добавлено N отсутствующих записей в index.md: [[slug3]]
- [x] Исправлено N полей frontmatter

### ТРЕБУЕТ ВНИМАНИЯ
- Страницы-сироты (нет входящих ссылок): [[slug1]], [[slug2]]
- Битые [[wikilinks]]: wiki/page.md → [[missing-slug]]
- Необработанные файлы в raw/: raw/articles/file.md, ...
- Устаревшие страницы (>6 мес.): [[slug1]] (последнее обновление: YYYY-MM-DD)

### СТАТИСТИКА
- Всего страниц в wiki: N
- Всего источников в raw/: N
- Страниц обновлено сегодня: N
```

If everything is clean, output: "Wiki в порядке ✓ — проблем не найдено."
