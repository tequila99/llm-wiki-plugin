Answer a question using the wiki knowledge base.

The question is: $ARGUMENTS

## Algorithm

### 1. Extract keywords

From the question, extract 3–6 search keywords (nouns, concepts, names).

All wiki pages are written in Russian, so search primarily in Russian. Also include English variants for technical terms that are commonly kept in original form in Russian text (e.g. "embedding", "fine-tuning", "backlink") — these may appear untranslated in wiki pages.

### 2. Find relevant pages

Run: `grep -ril "<keyword>" wiki/` for each keyword.

Combine results, deduplicate. Also read `wiki/index.md` — scan the table for rows whose tags or summary match the question.

Prioritize pages that appear in multiple keyword searches.

### 3. Read relevant pages

Read the identified pages. Maximum 15 pages per query to stay within context.

If more than 15 pages match, prefer:
- Pages matching the most keywords
- Pages with more recent `updated:` dates
- Pages linked from other matching pages

### 4. Synthesize answer

Write a clear, direct answer **in Russian**, regardless of the language the user asked in. Format:
- Lead with the direct answer
- Support with details from the wiki
- Cite sources as `[[wikilink]]` inline
- If the wiki doesn't have enough information, say so explicitly: "В wiki недостаточно информации по этому вопросу. Известно следующее: ..."

### 5. Offer to save

At the end, offer:
"Сохранить этот ответ как wiki-страницу? Если да, укажите название — он будет сохранён с тегом `query-answer`."

If the user confirms, create a wiki page:
```markdown
---
title: "<question as title>"
tags: [query-answer]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# <Question>

<The synthesized answer>

## See Also
<links to pages used in the answer>
```
Then update `wiki/index.md` and append to `wiki/log.md`.
