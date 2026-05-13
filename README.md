# dotclaude

**Take a screenshot. Type your question. That's it.**

`/snip` lets you show Claude something on your screen — without saving the picture, finding it later, or dragging files around.

Custom commands, skills, and prompts for [Claude Code](https://claude.com/claude-code) — with a `codex/` folder for [Codex CLI](https://github.com/openai/codex) ports as I write them.

Built by [@contactkvijay](https://github.com/contactkvijay). MIT licensed — fork, copy, remix.

## The problem

You see something on your screen — a bug, an error message, a weird-looking design — and you want Claude to look at it.

Without `/snip`, here's what you have to do every time:

1. Press **Win+Shift+S** and drag a box around what you want to show.
2. Click "Save" on the little notification that pops up (or paste it into Paint first).
3. Pick a folder. Pick a filename. Save it.
4. Open File Explorer. Hunt for the file you just saved.
5. Switch back to the window where Claude is running.
6. Drag the file into the chat box.
7. *Now* finally type your question.

Seven steps. By step five you've already forgotten what you wanted to ask. So most of the time you just skip the screenshot, try to describe the problem in words, and hope Claude can guess what you're looking at.

## How `/snip` fixes it

Same thing, with `/snip`:

1. Press **Win+Shift+S** and drag a box around what you want to show.
2. In Claude, type `/snip what's wrong here?` (or whatever you want to ask).
3. Claude saves the picture for you and answers your question.

Three steps. You never open File Explorer. You never pick a folder. You never name a file. The picture goes into a temp folder with a name like `snip-20260513-093200-742.png` — you don't have to remember where it is, and you don't have to clean it up.

## Demo

Win+Shift+S → `/snip describe this error` → Claude reads the picture and answers:

![/snip demo](./snip-command.gif)

## What's in here

| Command | What it does | Status |
|---|---|---|
| [`/snip`](./claude/commands/snip.md) | Save the current Windows clipboard image to a temp PNG and view it in the chat. Pair it with Win+Shift+S for screenshot-driven bug reports. | Claude ✅ &nbsp; Codex 🔜 |

More coming as I build them.

## Install (Claude Code)

Clone the repo, then copy or symlink the files into your `~/.claude/` directory.

### Windows (PowerShell)

```powershell
git clone https://github.com/contactkvijay/dotclaude.git
# Copy a single command into your user-scope Claude config:
Copy-Item .\dotclaude\claude\commands\snip.md $HOME\.claude\commands\snip.md
# Or copy everything:
Copy-Item -Recurse .\dotclaude\claude\* $HOME\.claude\
```

Restart Claude Code once. Then `/snip` is available in any project.

### macOS / Linux

```bash
git clone https://github.com/contactkvijay/dotclaude.git
ln -s "$PWD/dotclaude/claude/commands/snip.md" ~/.claude/commands/snip.md
# Or copy everything:
cp -r dotclaude/claude/* ~/.claude/
```

> **Project-scoped vs. user-scoped:** copying into `~/.claude/commands/` makes the command available everywhere. Copying into a project's `.claude/commands/` limits it to that project. Pick whichever fits.

## Install (Codex CLI)

Codex uses `~/.codex/prompts/*.md` (plain markdown, no YAML frontmatter, no per-command tool allowlist). Ports live under `codex/` here as I write them. The `snip` port is not done yet — coming after I verify it on a real Codex install.

## Is this Claude-only?

The *idea* of slash commands ports across CLIs; the *file format* doesn't.

- **Claude Code**: YAML frontmatter (`allowed-tools`, `argument-hint`), `$ARGUMENTS` substitution, project- or user-scoped under `.claude/commands/`.
- **Codex CLI**: plain markdown under `~/.codex/prompts/`, no frontmatter, different argument convention, shell access works through Codex's own sandboxing.

The PowerShell snippet inside `/snip` runs under either CLI — only the wrapper changes. That's why this repo separates `claude/` from `codex/` instead of mixing them.

## Contributing / forking

Open an issue or PR, or just fork it. If you adapt a command for another CLI (Cursor, Aider, etc.), open a PR with a new top-level folder — happy to host ports.
