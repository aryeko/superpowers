# Installing Superpowers for OpenCode

## Prerequisites

- [OpenCode.ai](https://opencode.ai) installed
- Git installed

## Installation Steps

### 1. Clone Superpowers

```bash
git clone https://github.com/aryeko/superpowers.git ~/.config/opencode/superpowers
```

### 2. Register the Plugin

Create a symlink so OpenCode discovers the plugin:

```bash
mkdir -p ~/.config/opencode/plugins
rm -f ~/.config/opencode/plugins/superpowers.js
ln -s ~/.config/opencode/superpowers/.opencode/plugins/superpowers.js ~/.config/opencode/plugins/superpowers.js
```

### 3. Symlink Skills

Create selective symlinks so OpenCode's native skill tool discovers only your OMO profile subset:

```bash
mkdir -p ~/.config/opencode/skills
rm -rf ~/.config/opencode/skills/superpowers

for skill in \
  brainstorming \
  systematic-debugging \
  verification-before-completion \
  test-driven-development \
  using-git-worktrees \
  finishing-a-development-branch \
  writing-skills \
  using-omo-superpowers
do
  rm -rf "$HOME/.config/opencode/skills/${skill}"
  ln -sfn "$HOME/.config/opencode/superpowers/skills/${skill}" "$HOME/.config/opencode/skills/${skill}"
done
```

### 4. Restart OpenCode

Restart OpenCode. The plugin will automatically inject superpowers context.

Skill IDs are namespaced (`superpowers/...`), and flat symlinks avoid duplicate entries like `/superpowers/superpowers/brainstorming`.

Verify by asking: "do you have superpowers?"

## Usage

### Finding Skills

Use OpenCode's native `skill` tool to list available skills:

```
use skill tool to list skills
```

### Loading a Skill

Use OpenCode's native `skill` tool to load a specific skill:

```
use skill tool to load superpowers/brainstorming
```

### Personal Skills

Create your own skills in `~/.config/opencode/skills/`:

```bash
mkdir -p ~/.config/opencode/skills/my-skill
```

Create `~/.config/opencode/skills/my-skill/SKILL.md`:

```markdown
---
name: my-skill
description: Use when [condition] - [what it does]
---

# My Skill

[Your skill content here]
```

### Project Skills

Create project-specific skills in `.opencode/skills/` within your project.

**Skill Priority:** Project skills > Personal skills > Superpowers skills

## Updating

```bash
cd ~/.config/opencode/superpowers
git pull
```

## Troubleshooting

### Plugin not loading

1. Check plugin symlink: `ls -l ~/.config/opencode/plugins/superpowers.js`
2. Check source exists: `ls ~/.config/opencode/superpowers/.opencode/plugins/superpowers.js`
3. Check OpenCode logs for errors

### Skills not found

1. Check selective links: `ls -l ~/.config/opencode/skills`
2. Verify each linked skill points into: `~/.config/opencode/superpowers/skills/`
3. Confirm bootstrap skill exists: `ls ~/.config/opencode/superpowers/skills/using-omo-superpowers/SKILL.md`
4. Use `skill` tool to list what's discovered

### Tool mapping

When skills reference Claude Code tools:
- `TodoWrite` → `update_plan`
- `Task` with subagents → `@mention` syntax
- `Skill` tool → OpenCode's native `skill` tool
- File operations → your native tools

## Getting Help

- Report issues: https://github.com/obra/superpowers/issues
- Full documentation: https://github.com/obra/superpowers/blob/main/docs/README.opencode.md
