Initialize the wiki knowledge base structure in the current directory.

## What to do

1. Check that `CLAUDE.md` exists in the current directory. If it doesn't, warn the user: "CLAUDE.md not found — the wiki schema is missing. Copy CLAUDE.md from the plugin directory into your wiki root and edit it to your preferences, then run /wiki-init again."

2. Create the following directories if they don't already exist:
   - `raw/articles/`
   - `raw/books/`
   - `raw/misc/`
   - `wiki/`
   - `.obsidian/`
   - `.claude/commands/`

3. Create `wiki/index.md` if it doesn't exist:
   ```markdown
   # Wiki Index

   | Страница | Теги | Резюме | Обновлено |
   |----------|------|--------|-----------|
   ```

4. Create `wiki/log.md` if it doesn't exist:
   ```markdown
   # Wiki Log

   ## YYYY-MM-DD — INIT
   - Wiki инициализирована
   ```
   (Replace YYYY-MM-DD with today's date.)

5. Create `.obsidian/app.json` if it doesn't exist:
   ```json
   {
     "useMarkdownLinks": false,
     "newFileLocation": "folder",
     "newFileFolderPath": "wiki",
     "attachmentFolderPath": "raw/misc"
   }
   ```

6. Report to the user:
   - Which directories were created (skip already-existing ones)
   - Which files were created (skip already-existing ones)
   - End with: "Wiki готова. Откройте папку в Obsidian как vault (File → Open folder as vault)."
