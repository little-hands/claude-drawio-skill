# draw-io

A Claude Code plugin for creating, reading, and editing draw.io (.drawio) diagrams.

## Features

Create, read, and edit diagrams in draw.io format (.drawio).

**Supported Diagram Types:**
- Architecture diagrams
- ER diagrams
- Flowcharts
- Wireframes
- Sequence diagrams
- Class diagrams
- Domain model diagrams
- Object diagrams

## Prerequisites

### draw.io Desktop App

draw.io desktop app must be installed.

**macOS:**

```bash
brew install --cask drawio
```

**Windows:**

```powershell
winget install -e --id JGraph.Draw
```

### AI Coding Tool

One of the following must be available:

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- [Cursor](https://www.cursor.com/) (Agent Skills supported)

## Installation

### Claude Code CLI (Terminal)

Launch `claude` in your terminal and run the following command.

```
/plugin install github:little-hands/claude-drawio-skill
```

### For Cursor Users

SKILL.md is an [open standard](https://www.mintlify.com/blog/skill-md) supported by multiple AI coding tools. Clone this repository and create a symbolic link in Cursor's global skills directory (`~/.cursor/skills/`).

1. Clone the repository (to any location):

   ```bash
   git clone https://github.com/little-hands/claude-drawio-skill.git
   ```

2. Create a symbolic link in the global skills directory:

   ```bash
   mkdir -p ~/.cursor/skills
   ln -s {claude-drawio-skill}/skills/draw-io ~/.cursor/skills/draw-io
   ```

   Replace `{claude-drawio-skill}` with the absolute path to the cloned repository.

   This makes the draw-io skill available across all projects. The AI reads the `description` frontmatter in `SKILL.md` and automatically loads the skill when it detects a relevant task (e.g., "create a diagram").

## Usage Examples

```text
"Create an architecture diagram"
"Create an ER diagram: User(id, name, email) → Order(id, user_id, total)"
"Open this .drawio file"
"Explain what's in the diagram"
"Add process D to the flowchart"
```

## Tips

### Recommended Settings

**Light Mode:**

Select "Extras → Appearance → Light". Dark mode causes color inversion issues in draw.io.

**Autosave:**

Enable "Extras → Autosave". When Claude Code edits a .drawio file, changes will be automatically reflected in the draw.io app.

### Google Drive Integration

Save .drawio files to Google Drive and sync locally with [Google Drive for Desktop](https://www.google.com/drive/download/) for convenient team sharing and version management.

## Skills

- `draw-io`: Create, edit, and read draw.io files

See [SKILL.md](./skills/draw-io/SKILL.md) for details.
