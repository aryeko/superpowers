# Superpowers for OpenCode

Complete guide for using Superpowers with [OpenCode.ai](https://opencode.ai).

## Quick Install

Tell OpenCode:

```
Clone https://github.com/aryeko/superpowers to ~/.config/opencode/superpowers, then create ~/.config/opencode/plugins and ~/.config/opencode/skills, then symlink ~/.config/opencode/superpowers/.opencode/plugins/superpowers.js to ~/.config/opencode/plugins/superpowers.js, then symlink only these skills into ~/.config/opencode/skills: brainstorming, systematic-debugging, verification-before-completion, test-driven-development, using-git-worktrees, finishing-a-development-branch, writing-skills, using-omo-superpowers, then restart opencode.
```

## Manual Installation

### Prerequisites

- [OpenCode.ai](https://opencode.ai) installed
- Git installed

### macOS / Linux

```bash
# 1. Install Superpowers (or update existing)
if [ -d ~/.config/opencode/superpowers ]; then
  cd ~/.config/opencode/superpowers && git pull
else
  git clone https://github.com/aryeko/superpowers.git ~/.config/opencode/superpowers
fi

# 2. Create directories
mkdir -p ~/.config/opencode/plugins ~/.config/opencode/skills

# 3. Remove old symlinks/directories if they exist
rm -f ~/.config/opencode/plugins/superpowers.js
rm -rf ~/.config/opencode/skills/superpowers

# 4. Create symlinks
ln -s ~/.config/opencode/superpowers/.opencode/plugins/superpowers.js ~/.config/opencode/plugins/superpowers.js
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
  ln -s "$HOME/.config/opencode/superpowers/skills/${skill}" "$HOME/.config/opencode/skills/${skill}"
done

# 5. Restart OpenCode
```

#### Verify Installation

```bash
ls -l ~/.config/opencode/plugins/superpowers.js
ls -l ~/.config/opencode/skills
```

Plugin should be a symlink, and `~/.config/opencode/skills/` should contain the selected skill links.

Skill IDs are namespaced as `superpowers/<skill-name>` to prevent duplicate command entries.

### Windows

**Prerequisites:**
- Git installed
- Either **Developer Mode** enabled OR **Administrator privileges**
  - Windows 10: Settings → Update & Security → For developers
  - Windows 11: Settings → System → For developers

Pick your shell below: [Command Prompt](#command-prompt) | [PowerShell](#powershell) | [Git Bash](#git-bash)

#### Command Prompt

Run as Administrator, or with Developer Mode enabled:

```cmd
:: 1. Install Superpowers
git clone https://github.com/aryeko/superpowers.git "%USERPROFILE%\.config\opencode\superpowers"

:: 2. Create directories
mkdir "%USERPROFILE%\.config\opencode\plugins" 2>nul
mkdir "%USERPROFILE%\.config\opencode\skills" 2>nul

:: 3. Remove existing links (safe for reinstalls)
del "%USERPROFILE%\.config\opencode\plugins\superpowers.js" 2>nul
rmdir "%USERPROFILE%\.config\opencode\skills\superpowers" 2>nul

:: 4. Create plugin symlink (requires Developer Mode or Admin)
mklink "%USERPROFILE%\.config\opencode\plugins\superpowers.js" "%USERPROFILE%\.config\opencode\superpowers\.opencode\plugins\superpowers.js"

:: 5. Create selective skill junctions (works without special privileges)
for %%S in (brainstorming systematic-debugging verification-before-completion test-driven-development using-git-worktrees finishing-a-development-branch writing-skills using-omo-superpowers) do (
  rmdir "%USERPROFILE%\.config\opencode\skills\%%S" 2>nul
  del "%USERPROFILE%\.config\opencode\skills\%%S" 2>nul
  mklink /J "%USERPROFILE%\.config\opencode\skills\%%S" "%USERPROFILE%\.config\opencode\superpowers\skills\%%S"
)

:: 6. Restart OpenCode
```

#### PowerShell

Run as Administrator, or with Developer Mode enabled:

```powershell
# 1. Install Superpowers
git clone https://github.com/aryeko/superpowers.git "$env:USERPROFILE\.config\opencode\superpowers"

# 2. Create directories
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.config\opencode\plugins"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.config\opencode\skills"

# 3. Remove existing links (safe for reinstalls)
Remove-Item "$env:USERPROFILE\.config\opencode\plugins\superpowers.js" -Force -ErrorAction SilentlyContinue
Remove-Item "$env:USERPROFILE\.config\opencode\skills\superpowers" -Force -ErrorAction SilentlyContinue

# 4. Create plugin symlink (requires Developer Mode or Admin)
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.config\opencode\plugins\superpowers.js" -Target "$env:USERPROFILE\.config\opencode\superpowers\.opencode\plugins\superpowers.js"

# 5. Create selective skill junctions (works without special privileges)
$skills = @("brainstorming","systematic-debugging","verification-before-completion","test-driven-development","using-git-worktrees","finishing-a-development-branch","writing-skills","using-omo-superpowers")
foreach ($skill in $skills) {
  Remove-Item "$env:USERPROFILE\.config\opencode\skills\$skill" -Force -ErrorAction SilentlyContinue
  New-Item -ItemType Junction -Path "$env:USERPROFILE\.config\opencode\skills\$skill" -Target "$env:USERPROFILE\.config\opencode\superpowers\skills\$skill"
}

# 6. Restart OpenCode
```

#### Git Bash

Note: Git Bash's native `ln` command copies files instead of creating symlinks. Use `cmd //c mklink` instead (the `//c` is Git Bash syntax for `/c`).

```bash
# 1. Install Superpowers
git clone https://github.com/aryeko/superpowers.git ~/.config/opencode/superpowers

# 2. Create directories
mkdir -p ~/.config/opencode/plugins ~/.config/opencode/skills

# 3. Remove existing links (safe for reinstalls)
rm -f ~/.config/opencode/plugins/superpowers.js 2>/dev/null
rm -rf ~/.config/opencode/skills/superpowers 2>/dev/null

# 4. Create plugin symlink (requires Developer Mode or Admin)
cmd //c "mklink \"$(cygpath -w ~/.config/opencode/plugins/superpowers.js)\" \"$(cygpath -w ~/.config/opencode/superpowers/.opencode/plugins/superpowers.js)\""

# 5. Create selective skill junctions (works without special privileges)
for skill in brainstorming systematic-debugging verification-before-completion test-driven-development using-git-worktrees finishing-a-development-branch writing-skills using-omo-superpowers; do
  cmd //c "rmdir \"$(cygpath -w ~/.config/opencode/skills/$skill)\" 2>nul"
  cmd //c "del \"$(cygpath -w ~/.config/opencode/skills/$skill)\" 2>nul"
  cmd //c "mklink /J \"$(cygpath -w ~/.config/opencode/skills/$skill)\" \"$(cygpath -w ~/.config/opencode/superpowers/skills/$skill)\""
done

# 6. Restart OpenCode
```

#### WSL Users

If running OpenCode inside WSL, use the [macOS / Linux](#macos--linux) instructions instead.

#### Verify Installation

**Command Prompt:**
```cmd
dir /AL "%USERPROFILE%\.config\opencode\plugins"
dir /AL "%USERPROFILE%\.config\opencode\skills"
```

**PowerShell:**
```powershell
Get-ChildItem "$env:USERPROFILE\.config\opencode\plugins" | Where-Object { $_.LinkType }
Get-ChildItem "$env:USERPROFILE\.config\opencode\skills" | Where-Object { $_.LinkType }
```

Look for `<SYMLINK>` or `<JUNCTION>` in the output.

#### Troubleshooting Windows

**"You do not have sufficient privilege" error:**
- Enable Developer Mode in Windows Settings, OR
- Right-click your terminal → "Run as Administrator"

**"Cannot create a file when that file already exists":**
- Run the removal commands (step 3) first, then retry

**Symlinks not working after git clone:**
- Run `git config --global core.symlinks true` and re-clone

## Usage

### Finding Skills

Use OpenCode's native `skill` tool to list all available skills:

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

Create project-specific skills in your OpenCode project:

```bash
# In your OpenCode project
mkdir -p .opencode/skills/my-project-skill
```

Create `.opencode/skills/my-project-skill/SKILL.md`:

```markdown
---
name: my-project-skill
description: Use when [condition] - [what it does]
---

# My Project Skill

[Your skill content here]
```

## Skill Locations

OpenCode discovers skills from these locations:

1. **Project skills** (`.opencode/skills/`) - Highest priority
2. **Personal skills** (`~/.config/opencode/skills/`)
3. **Selected Superpowers skills** (`~/.config/opencode/skills/<skill-link>`) - via symlink/junction

## Features

### Automatic Context Injection

The plugin automatically injects superpowers context via the `experimental.chat.system.transform` hook. This adds the "using-omo-superpowers" skill content to the system prompt on every request.

### Native Skills Integration

Superpowers uses OpenCode's native `skill` tool for skill discovery and loading. Selected skills are linked directly into `~/.config/opencode/skills/` so they appear alongside your personal and project skills.

### Tool Mapping

Skills written for Claude Code are automatically adapted for OpenCode. The bootstrap provides mapping instructions:

- `TodoWrite` → `update_plan`
- `Task` with subagents → OpenCode's `@mention` system
- `Skill` tool → OpenCode's native `skill` tool
- File operations → Native OpenCode tools

## Architecture

### Plugin Structure

**Location:** `~/.config/opencode/superpowers/.opencode/plugins/superpowers.js`

**Components:**
- `experimental.chat.system.transform` hook for bootstrap injection
- Reads and injects the "using-omo-superpowers" skill content

### Skills

**Location:** `~/.config/opencode/skills/` (selective symlinks/junctions to chosen directories under `~/.config/opencode/superpowers/skills/`)

Skills are discovered by OpenCode's native skill system. Each skill has a `SKILL.md` file with YAML frontmatter.

## Updating

```bash
cd ~/.config/opencode/superpowers
git pull
```

Restart OpenCode to load the updates.

## Troubleshooting

### Plugin not loading

1. Check plugin exists: `ls ~/.config/opencode/superpowers/.opencode/plugins/superpowers.js`
2. Check symlink/junction: `ls -l ~/.config/opencode/plugins/` (macOS/Linux) or `dir /AL %USERPROFILE%\.config\opencode\plugins` (Windows)
3. Check OpenCode logs: `opencode run "test" --print-logs --log-level DEBUG`
4. Look for plugin loading message in logs

### Skills not found

1. Verify selective skill links: `ls -l ~/.config/opencode/skills`
2. Use OpenCode's `skill` tool to list available skills
3. Check skill structure: each skill needs a `SKILL.md` file with valid frontmatter

### Duplicate skills shown in picker

If you see `/superpowers/superpowers/<skill>`, remove legacy nested links (`rm -rf ~/.config/opencode/skills/superpowers`) and recreate flat links in `~/.config/opencode/skills`, then restart OpenCode.

### Windows: Module not found error

If you see `Cannot find module` errors on Windows:
- **Cause:** Git Bash `ln -sf` copies files instead of creating symlinks
- **Fix:** Use `mklink /J` directory junctions instead (see Windows installation steps)

### Bootstrap not appearing

1. Verify using-omo-superpowers skill exists: `ls ~/.config/opencode/superpowers/skills/using-omo-superpowers/SKILL.md`
2. Check OpenCode version supports `experimental.chat.system.transform` hook
3. Restart OpenCode after plugin changes

## Getting Help

- Report issues: https://github.com/obra/superpowers/issues
- Main documentation: https://github.com/obra/superpowers
- OpenCode docs: https://opencode.ai/docs/

## Testing

Verify your installation:

```bash
# Check plugin loads
opencode run --print-logs "hello" 2>&1 | grep -i superpowers

# Check skills are discoverable
opencode run "use skill tool to list all skills" 2>&1 | grep -i superpowers

# Check bootstrap injection
opencode run "what superpowers do you have?"
```

The agent should mention having superpowers and be able to list skills from `superpowers/`.
