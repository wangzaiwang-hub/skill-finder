# Skill Finder

> 🔍 Search and install skills from the [skills.dog](https://skills.dog) marketplace directly in Claude Code

## Features

- 🔎 **Semantic Search** - Find skills using natural language descriptions
- 📚 **Browse by Category** - Explore skills by coding, debugging, testing, deploying, etc.
- ⭐ **Discover Popular Skills** - See what the community loves
- 📦 **One-Click Install** - Download and set up skills with a single command
- 🌍 **Global or Local** - Install skills system-wide or per-project

## Quick Start

### Search for a Skill

```
帮我找个下载视频的 skill
```

or

```
find a skill for code review
```

### Browse Popular Skills

```
看看热门 skills
```

or

```
show me the most popular skills
```

### Browse by Category

```
show me coding skills
```

Available categories: `coding`, `debugging`, `testing`, `reviewing`, `shipping`, `deploying`, `documenting`, `designing`, `analyzing`, `automating`

## How It Works

### 1. Search

When you describe what you need, Skill Finder:
- Extracts keywords from your request
- Performs semantic search on skills.dog
- Shows top 5 matching skills with ratings

### 2. Select

- Choose from the search results
- Or browse by popularity/category

### 3. Install

- Choose installation location:
  - **Global** (`~/.claude/skills/`) - Available to all projects
  - **Current directory** (`./skills/`) - Only for this project
- Skill files are downloaded and set up automatically

### 4. Use

Start using your new skill immediately with `/skill-name` or natural language!

## Example Session

```
User: 帮我找个测试的 skill

Skill Finder: Found 5 skills:

1. **test-driven-development** by superpowers ⭐ 245
   Use when implementing any feature or bugfix, before writing implementation code

2. **systematic-debugging** by superpowers ⭐ 198
   Use when encountering any bug, test failure, or unexpected behavior

3. ...

User: Install #1

Skill Finder: Where would you like to install?
- Global (~/.claude/skills/)
- Current directory (./skills/)

User: Global

Skill Finder: ✓ Installed test-driven-development to ~/.claude/skills/test-driven-development/

Usage: /test-driven-development or describe your testing needs
```

## API Integration

This skill integrates with the skills.dog API:

| Endpoint | Purpose |
|----------|---------|
| `/api/semantic-search` | Search skills by meaning |
| `/api/skills` | Browse skills (by popularity, category, latest) |
| `/api/download/{id}/json` | Download skill files |

## Development

This is a Claude Code skill. The skill logic is defined in `SKILL.md` following the [skill creation guidelines](https://docs.cursor.com/context/agents/skills).

### File Structure

```
skill-finder/
├── SKILL.md       # Skill logic and workflow
└── README.md      # This file
```

## Contributing

Found a bug or have a feature request? Please open an issue on the repository.

## License

MIT

---

**Made with ❤️ for the Claude Code community**
