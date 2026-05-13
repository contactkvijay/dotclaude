# dotclaude

**Show, don't describe.**

The fastest way to ask Claude about anything you can see.

You're staring at a weird error. A broken layout. A chart that doesn't add up. You *could* try to put it into words. Or you could just show.

`/snip` makes showing as easy as asking.

![/snip demo](./snip-command.gif)

## Before `/snip`

You see something you want Claude to look at. Here's the dance:

1. **Win+Shift+S.** Drag a box around it.
2. Click "Save" on the little notification.
3. Pick a folder. Type a filename. Save.
4. Open File Explorer. Hunt for the file.
5. Switch back to Claude.
6. Drag the file into the chat box.
7. *Now* type your question.

Seven steps. By step five, you've forgotten what you wanted to ask.

So most days, you skip the screenshot. You try to describe the bug in words. Claude guesses. You both waste time.

## After `/snip`

1. **Win+Shift+S.** Drag a box around it.
2. Type `/snip what's wrong here?`
3. Claude saves the picture and answers.

That's it.

No folder to pick. No filename to invent. No dragging. The picture lands in a temp folder with a timestamp name like `snip-20260513-093200-742.png` — out of sight, out of mind.

## Install

Windows only. Requires [Claude Code](https://claude.com/claude-code).

```powershell
git clone https://github.com/contactkvijay/dotclaude.git
Copy-Item .\dotclaude\claude\commands\snip.md $HOME\.claude\commands\snip.md
```

Restart Claude Code. `/snip` is now available in every project.

---

Built by [@contactkvijay](https://github.com/contactkvijay). MIT licensed.
