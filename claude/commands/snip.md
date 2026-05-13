---
description: Save the current Windows clipboard image to a temp PNG and view it
argument-hint: [optional-label]
allowed-tools: PowerShell, Read
---

Save the current Windows clipboard image to `$env:TEMP\claude-snips\` and view it. Windows-only — uses `System.Windows.Forms.Clipboard.GetImage()` to read the clipboard image, which is what Snipping Tool (Win+Shift+S) writes.

The user typed: `/snip $ARGUMENTS`

Steps:

1. Run this PowerShell snippet exactly as written. It writes the clipboard image to a timestamped PNG under `$env:TEMP\claude-snips\` and prints the absolute path to stdout. If the clipboard has no image, it exits non-zero with an error message.

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
$raw = @'
$ARGUMENTS
'@
$label = ($raw.Trim() -replace '[^A-Za-z0-9_-]', '-').Trim('-')
if (-not $label) { $label = 'snip' }
$stamp = Get-Date -Format 'yyyyMMdd-HHmmss'
$path = Join-Path $dir "$label-$stamp.png"
$img.Save($path, [System.Drawing.Imaging.ImageFormat]::Png)
$img.Dispose()
Write-Output $path
```

2. If the snippet succeeded, use the Read tool on the absolute path it printed. The PNG will load as an image you can see.

3. In one sentence, acknowledge the filename and describe what's in the screenshot so the user knows it loaded correctly.

4. If the snippet failed (no image on clipboard, or any other error), tell the user verbatim: "No image found on the clipboard. Take a screenshot with Win+Shift+S, then re-run /snip." Do not retry the snippet — wait for the user.
