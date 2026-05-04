Ingest a new source into the wiki knowledge base.

The argument (`$ARGUMENTS`) can be:
- A file path inside `raw/` (e.g. `raw/articles/my-article.md`)
- A URL — fetch, save to `raw/articles/YYYY-MM-DD-<slug>.md` with `url:` and `collected:` frontmatter
- Plain text — save to `raw/misc/YYYY-MM-DD-manual.md`

## Algorithm

### 1. Acquire the source
Read the file, fetch the URL, or save the plain text. Then read the full content.

### 2. Extract key ideas
Extract **5–12 key takeaways** in Russian. Show them as a numbered list to the user, then immediately continue — no questions asked.

### 3. Deduplication
Read `wiki/index.md`. For existing topics: update. For new topics: create.

### 4. Write wiki pages (typically 5–15 per source)
- Write entirely in Russian; keep untranslatable terms in original with a brief explanation on first use
- Frontmatter: `title` (Russian), `tags`, `created`, `updated` (today), `sources`
- Body: synthesized insights, not a summary
- Always include `## See Also` with `[[wikilinks]]` to related pages
- File name: `wiki/lowercase-hyphenated-slug.md`

### 5. Backlink audit (CRITICAL)
For every created/updated page, scan all `wiki/*.md` files. Where the new page is relevant and not yet linked, add `[[new-slug]]` to the `## See Also` section and update `updated:` to today.

### 6. Update `wiki/index.md`
One row per created/updated page: `| [[slug]] | tags | Одно предложение резюме | YYYY-MM-DD |`

### 7. Append to `wiki/log.md`
```
## YYYY-MM-DD HH:MM — INGEST
- Источник: `path/to/file`
- Создано: [[p1]], [[p2]]
- Обновлено: [[p3]]
- Backlinks добавлены в: [[p4]], [[p5]]
```

### 8. Report
List created pages, updated pages, and backlinks added.
