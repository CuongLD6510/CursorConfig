# Copy or Symlink the Cursor Config Bundle

This guide explains how to copy or symlink the `.cursor/` folder from this repo to your **user Cursor directory** (so all projects use it) or to a **single project** (so only that project uses it).

## Paths

| Target | Windows | macOS / Linux |
|--------|---------|----------------|
| User Cursor dir | `%APPDATA%\Cursor` | `~/.cursor` (macOS/Linux: `~/.config/Cursor` or `~/.cursor` depending on Cursor version) |
| Project | `<project-root>\.cursor` | `<project-root>/.cursor` |

Use the path Cursor actually uses on your machine (e.g. on Windows it is often `%APPDATA%\Cursor`).

---

## Option A: User-level (all projects)

Apply the bundle to your user Cursor directory so every project can use the same commands, rules, skills, and subagents.

### Windows (PowerShell)

**Copy (recommended if you want to merge with existing .cursor):**

```powershell
$source = "D:\CuongLD\WorkSpace\projects\CursorConfig\.cursor"
$dest   = "$env:APPDATA\Cursor"
# Copy each subfolder; merge with existing
New-Item -ItemType Directory -Force -Path "$dest\commands" | Out-Null
New-Item -ItemType Directory -Force -Path "$dest\rules"   | Out-Null
New-Item -ItemType Directory -Force -Path "$dest\skills" | Out-Null
New-Item -ItemType Directory -Force -Path "$dest\agents" | Out-Null
Copy-Item -Path "$source\commands\*" -Destination "$dest\commands" -Recurse -Force
Copy-Item -Path "$source\rules\*"   -Destination "$dest\rules"   -Recurse -Force
Copy-Item -Path "$source\skills\*"  -Destination "$dest\skills"  -Recurse -Force
Copy-Item -Path "$source\agents\*"  -Destination "$dest\agents"  -Recurse -Force
```

**Symlink (single folder; replaces existing `.cursor` if you link the whole folder):**

```powershell
# Run PowerShell as Administrator if you get "privilege" errors
$source = "D:\CuongLD\WorkSpace\projects\CursorConfig\.cursor"
$dest   = "$env:APPDATA\Cursor"
New-Item -ItemType SymbolicLink -Path "$dest\.cursor-bundle" -Target $source -Force
# Then merge: copy contents of .cursor-bundle into $dest (commands, rules, etc.)
# Or, to replace entire .cursor for user:
# Remove-Item "$dest\.cursor" -Recurse -Force -ErrorAction SilentlyContinue
# New-Item -ItemType SymbolicLink -Path "$dest\.cursor" -Target $source -Force
```

Adjust `$source` to the actual path of the CursorConfig repo on your machine.

### macOS / Linux (bash/zsh)

**Copy:**

```bash
SOURCE="$HOME/path/to/CursorConfig/.cursor"
DEST="$HOME/.cursor"   # or ~/.config/Cursor if Cursor uses that

mkdir -p "$DEST"/{commands,rules,skills,agents}
cp -R "$SOURCE"/commands/* "$DEST/commands/"
cp -R "$SOURCE"/rules/*   "$DEST/rules/"
cp -R "$SOURCE"/skills/*  "$DEST/skills/"
cp -R "$SOURCE"/agents/*  "$DEST/agents/"
```

**Symlink (entire .cursor):**

```bash
# Backup existing .cursor if you have one
mv ~/.cursor ~/.cursor.bak
ln -s "$HOME/path/to/CursorConfig/.cursor" ~/.cursor
```

Replace `$HOME/path/to/CursorConfig` with the real path to this repo.

---

## Option B: Per-project

Apply the bundle only to one project by putting `.cursor` inside that project's root.

### Windows (PowerShell)

**Copy:**

```powershell
$source = "D:\CuongLD\WorkSpace\projects\CursorConfig\.cursor"
$projectRoot = "C:\path\to\your\project"
$dest = "$projectRoot\.cursor"

New-Item -ItemType Directory -Force -Path $dest | Out-Null
Copy-Item -Path "$source\*" -Destination $dest -Recurse -Force
```

**Symlink:**

```powershell
# Run from project root; run as Administrator if needed
$source = "D:\CuongLD\WorkSpace\projects\CursorConfig\.cursor"
cd C:\path\to\your\project
New-Item -ItemType SymbolicLink -Path ".cursor" -Target $source -Force
```

### macOS / Linux

**Copy:**

```bash
SOURCE="$HOME/path/to/CursorConfig/.cursor"
PROJECT="/path/to/your/project"
cp -R "$SOURCE" "$PROJECT/.cursor"
```

**Symlink:**

```bash
cd /path/to/your/project
ln -s "$HOME/path/to/CursorConfig/.cursor" .cursor
```

---

## After copying or symlinking

1. Restart Cursor (or reload the window) so it picks up new commands and rules.
2. In a project, run `/create-config-for-this-project @current-project` to generate project-specific rules and optional config.
3. Use `/task`, `/build`, `/commit`, etc. as needed.

## Undo

- **Copy**: Delete or overwrite the copied files/folders in `%APPDATA%\Cursor` or in the project's `.cursor/`.
- **Symlink**: Remove the symlink (e.g. `Remove-Item .cursor` or `rm .cursor`) and, if you had a backup, restore it (e.g. `mv ~/.cursor.bak ~/.cursor`).
