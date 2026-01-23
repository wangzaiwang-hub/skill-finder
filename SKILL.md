---
name: skill-finder
description: Search and install skills from skills.dog marketplace
---

# Skill Finder

Search and install skills from the skills.dog marketplace directly in Claude Code.

## Trigger

- Command: `/skill-finder` or `/sf`
- Natural language: "帮我找个 xxx 的 skill", "find a skill for xxx"

## Workflow

### 1. Search Mode

When user describes a need (e.g., "帮我找个下载视频的 skill"):

1. Extract keywords from user request
2. Call semantic search API:
   ```
   WebFetch https://skills.dog/api/semantic-search?q={keywords}&limit=5
   ```
3. **Parse response** - each skill has these fields (SAVE the `id` for download):
   ```json
   {
     "skills": [
       {
         "id": "019abc12-3456-7def-...",  // UUID v7, REQUIRED for download
         "name": "skill-name",
         "author": "author-name",
         "description": "...",
         "stars": 100
       }
     ]
   }
   ```
4. Display top 5 results in format:
   ```
   Found X skills:

   1. **{name}** by {author} ⭐ {stars}
      {description}

   2. ...
   ```
5. Use AskUserQuestion to let user select which to install (options: 1-5 + "Cancel")
6. **IMPORTANT**: Remember the selected skill's `id` field for the download step

### 2. Browse Mode

When user wants to explore (e.g., "看看热门 skills", "what coding skills are there"):

- Popular: `GET https://skills.dog/api/skills?sort=stars&limit=10`
- By category: `GET https://skills.dog/api/skills?category={category}&limit=10`
- Latest: `GET https://skills.dog/api/skills?sort=latest&limit=10`

Categories: coding, debugging, testing, reviewing, shipping, deploying, documenting, designing, analyzing, automating

**Response format** (same as search, SAVE the `id` for download):
```json
{
  "skills": [{ "id": "019abc12-...", "name": "...", "author": "...", ... }]
}
```

### 3. Install

After user selects a skill:

1. **Ask install location** using AskUserQuestion:
   - "Global (~/.claude/skills/)" - available to all projects
   - "Current directory (./skills/)" - only for this project

2. **Fetch skill files** using the `id` from search/browse results:
   ```
   WebFetch https://skills.dog/api/download/{id}/json
   prompt: "Extract the JSON response"
   ```

   **CRITICAL**: Use the UUID `id` field (e.g., `019abc12-3456-7def-...`), NOT author/name!
   - ✅ Correct: `/api/download/019abc12-3456-7def-.../json`
   - ❌ Wrong: `/api/download/author/skill-name/json`

3. **Parse response** - WebFetch returns JSON directly:
   ```json
   {
     "id": "xxx",
     "name": "skill-name",
     "author": "author-name",
     "files": [
       {"path": "SKILL.md", "content": "..."},
       {"path": "scripts/setup.sh", "content": "..."}
     ]
   }
   ```

4. **Create directory** based on user choice:
   - Global: `mkdir -p ~/.claude/skills/{name}/`
   - Current: `mkdir -p ./skills/{name}/`

5. **Write files** - iterate `files` array, use Write tool for each:
   ```
   for each file in files:
     Write({install_path}/{file.path}, file.content)
   ```

6. **Confirm success**:
   ```
   ✓ Installed {name} to {install_path}

   Usage: /{name} or describe your task
   ```

### 4. Conflict Handling

If target directory exists, use AskUserQuestion:
- Overwrite
- Skip
- Rename (e.g., skill-name-2)

## API Reference

Base URL: https://skills.dog

| Endpoint | Description |
|----------|-------------|
| GET /api/semantic-search?q={query}&limit=5 | Semantic search skills |
| GET /api/skills?sort=stars&limit=10 | List by popularity |
| GET /api/skills?category={cat}&limit=10 | List by category |
| GET /api/download/{id}/json | Get skill files (id = UUID from search results) |
