# 🎭 xdg-mimic

> **⚠️ Status: Experimental**

A smart, lightweight shadow for `xdg-mime` that brings dynamic "Open With..." menus to standalone window managers (Hyprland, Sway, i3, etc.). Supports fuzzel, rofi, wofi, tofi, bemenu & dmenu.

## 📖 The Philosophy

If you use a standalone window manager instead of a full Desktop Environment like GNOME or KDE, `xdg-open` falls back to its "generic" mode. Generic mode is dumb: if a default app is missing, deleted, or unassigned, it either fails silently or guesses a random terminal app. 

Other projects in the ecosystem (like `handlr-regex`) solve this by acting as a complete replacement for `xdg-open`.

**`xdg-mimic` takes a different approach:**
Why replace `xdg-open` when it already has perfectly good fallback and URL parsing logic built-in? Instead of a replacement, this project is a simple Bash script that **shadows** the underlying `xdg-mime` executable. 

By placing `xdg-mimic` ahead of the system `xdg-mime` in your `$PATH`, we intercept `xdg-open` right at the moment it asks for a default application. If the default is missing or deleted, we intercept the failure, instantly pop up a dynamic launcher menu for you to pick an app, save your choice, and seamlessly hand it right back to `xdg-open`.

## ⚙️ Configuration

Open the `xdg-mime` script in your favorite text editor to configure the launcher it uses:

```bash
# Set to "fuzzel", "rofi", "wofi", "tofi", "bemenu", or "dmenu" to force a specific launcher. 
# Leave empty to auto-detect!
LAUNCHER_OVERRIDE=""
```
By default, the script will automatically scan your system and pick the best available launcher. If you have multiple installed, you can force your preferred one by typing its name in the quotes!

## 🚀 Installation

Simply download the script, name it `xdg-mime`, and place it somewhere in your `$PATH` that takes priority over `/usr/bin/` (like `~/.local/bin/`).

```bash
mkdir -p ~/.local/bin
curl -o ~/.local/bin/xdg-mime https://raw.githubusercontent.com/elseawhy/xdg-mimic/refs/heads/main/xdg-mime
chmod +x ~/.local/bin/xdg-mime
```

Ensure `~/.local/bin` is in your `$PATH`. The next time you use `xdg-open` on an unknown file, your launcher will pop up!

## 🤝 Contributing
Since this is experimental, feel free to open issues or PRs if you find edge cases with specific file types or desktop environments!
