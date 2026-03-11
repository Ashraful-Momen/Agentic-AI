# Claude Code CLI Installation - Linux Cheat Sheet

## Quick Install

```bash
# Using npm (Node.js required)
sudo npm install -g @anthropic-ai/claude-code

# Or using curl
curl -fsSL https://cdn.jsdelivr.net/npm/@anthropic-ai/claude-code/install.sh | sh
```

## Prerequisites
- **Node.js** 18+ or **Bun** 1.0+
- **Git** (for repo operations)
- **API Key** from [Anthropic Console](https://console.anthropic.com/)

## First-Time Setup

```bash
# Authenticate
claude auth login

# Verify installation
claude --version
```

## Basic Commands

| Command | Description |
|---------|-------------|
| `claude` | Start interactive session |
| `claude --help` | Show all commands |
| `claude auth status` | Check authentication |
| `claude config set` | Set configuration options |

## Common Options

```bash
# Specify model
claude --model claude-sonnet-4-6

# Work in specific directory
claude --cwd /path/to/project

# Auto-approve all actions
claude --auto-approve
```

## Setup GLM 5

```bash
# goto claude setting
cd ~/.claude/

# make claude settings.json
nano settings.json

# save those config
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "b6c38d64274c4763bad0288a9c52ad83.nXE23gKO6Bms2xWr",
    "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "API_TIMEOUT_MS": "3000000"
  },
  "model": "glm-5"
}
```


## Project Configuration Files

### CLAUDE.md - Project Instructions
Create `.claude/CLAUDE.md` in your project root to give Claude context about your project:

```markdown
# Project Overview
This is a Laravel e-commerce application.

# Important Conventions
- Follow PSR-12 coding standards
- Use Laravel's built-in features
- Always write tests for new features

# Common Tasks
- Run tests: `php artisan test`
- Start server: `php artisan serve`
```

### SKILLS.md - Custom Commands
Create `.claude/SKILLS.md` to define project-specific commands:

```markdown
---
description: Run Laravel tests
---

Runs the test suite and reports results.

```bash
php artisan test --parallel
```

---
description: Clear Laravel cache
---

Clears all Laravel caches.

```bash
php artisan cache:clear && php artisan config:clear && php artisan view:clear
```
```

### AGENT.md - Agent Configuration
Create `.claude/AGENT.md` to customize agent behavior:

```markdown
You are a Laravel expert. Always:
- Check existing patterns before writing new code
- Use Laravel's coding standards
- Consider security implications
- Test your changes
```

## Important Commands Reference

### Slash Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `/help` | Show available commands | Type anytime |
| `/clear` | Clear conversation history | Start fresh |
| `/commit` | Create git commit | `/commit -m "message"` |
| `/init` | Initialize project | Sets up CLAUDE.md |
| `/fast` | Toggle fast mode | Faster output |
| `/tasks` | Show background tasks | List running tasks |

### Working with Claude

```bash
# Start in specific directory
claude --cwd /path/to/project

# Use specific model
claude --model claude-opus-4-6

# Read-only mode (no file changes)
claude --read-only

# Diff mode (show changes before applying)
claude --diff
```

### Tips for Efficient Usage

1. **Be Specific**: Clear requests = better results
   - ❌ "Fix the code"
   - ✅ "Fix the SQL injection vulnerability in user login"

2. **Use Selection**: Select code in IDE before asking
   - Claude knows what you're looking at

3. **Provide Context**: Give background when needed
   - "This is a Laravel project using PHP 8.2"

4. **Iterate**: Build complex features step by step
   - Start with basic implementation
   - Add features incrementally

5. **Use Skills**: Create reusable commands for common tasks
   - Define in SKILLS.md
   - Call with `/skill-name`

6. **Memory**: Claude remembers across sessions
   - Stored in `.claude/memory/`
   - Organize by topic

## Get Help
- `claude --help` - Full command reference
- [GitHub Repo](https://github.com/anthropics/claude-code) - Documentation & Issues
