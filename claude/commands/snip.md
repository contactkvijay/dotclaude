---
description: Save the current Windows clipboard screenshot to a temp PNG and respond to the user about it
argument-hint: [your question or context about the screenshot]
allowed-tools: PowerShell, Read
---

The user just took a screenshot with Win+Shift+S — it's on the Windows clipboard, not yet saved to disk. They want you to (a) save it so you can see it, and (b) answer whatever they wrote after `/snip` using the screenshot as visual context.

The user's request:

`$ARGUMENTS`

Treat the text above as their actual question / instruction. The screenshot is the visual input that goes with it. If `$ARGUMENTS` is empty, just describe what's in the screenshot so they know it loaded.

Steps:

1. Run this PowerShell snippet exactly as written. It writes the clipboard image to a timestamped PNG under `$env:TEMP\claude-snips\` and prints the absolute path to stdout. The filename is just a timestamp — the user's description is NOT encoded into it. If the clipboard has no image, the snippet exits non-zero with an error message.

```powershell
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing
$img = [System.Windows.Forms.Clipboard]::GetImage()
if ($null -eq $img) {
  Write-Error 'No image on the clipboard. Take a screenshot with Win+Shift+S, then re-run /snip.'
  exit 1
}
$dir = Join-Path $env:TEMP 'claude-snips'
New-Item -ItemType Directory -Force -Path $dir | Out-Null
$stamp = Get-Date -Format 'yyyyMMdd-HHmmss-fff'
$path = Join-Path $dir "snip-$stamp.png"
$img.Save($path, [System.Drawing.Imaging.ImageFormat]::Png)
$img.Dispose()
Write-Output $path
```

2. If the snippet succeeded, use the Read tool on the absolute path it printed. The PNG will load as an image you can see.

3. Now respond to the user's request (the `$ARGUMENTS` text shown above), using the screenshot as your visual context. Do not just describe the image — actually answer their question or do what they asked. If they passed no arguments, then a one-sentence description of what's in the screenshot is the right response.

4. If the snippet failed (no image on clipboard, or any other error), tell the user verbatim: "No image found on the clipboard. Take a screenshot with Win+Shift+S, then re-run /snip." Do not retry the snippet — wait for the user.
