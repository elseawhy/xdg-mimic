# 🎭 xdg-mimic

<video src="https://github.com/user-attachments/assets/d3f00883-0480-4630-a404-6eb262a2941d" controls="controls" width="100%"></video>

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

### Always show the menu

By default, the menu only appears when the default app is missing. To pick an app every time, even for known files, list MIME type globs in `ALWAYS_ASK`:

```bash
ALWAYS_ASK=("image/*" "video/*" "application/pdf")
```

The current default app is listed first, so Enter or Esc opens it as usual. A choice made from this menu is not saved as the new default.

To show the menu for a single file only, use `xdg-open --ask` (or `-a`). This is handy as an "Open With..." keybind or file manager action:

```bash
xdg-open --ask file.pdf
```

This needs the optional `xdg-open` wrapper (see [Installation](#-installation)), because the real `xdg-open` does not accept extra options. The wrapper does not replace `xdg-open`: it removes `--ask`, marks this one call, and runs `/usr/bin/xdg-open` with the remaining arguments. The app that opens does not inherit the request, so files and links that it opens later use the default app as usual.

The menu only appears when `xdg-open` is the caller. Other programs that query `xdg-mime` still get the default app silently. Note that `"*"` also matches `x-scheme-handler/*`, so every link you click will ask too.

## 🚀 Installation

Simply download the script, name it `xdg-mime`, and place it somewhere in your `$PATH` that takes priority over `/usr/bin/` (like `~/.local/bin/`).

```bash
mkdir -p ~/.local/bin
curl -o ~/.local/bin/xdg-mime https://raw.githubusercontent.com/elseawhy/xdg-mimic/refs/heads/main/xdg-mime
chmod +x ~/.local/bin/xdg-mime
```

Ensure `~/.local/bin` is in your `$PATH`. The next time you use `xdg-open` on an unknown file, your launcher will pop up!

To use `xdg-open --ask`, also install the `xdg-open` wrapper in the same directory:

```bash
curl -o ~/.local/bin/xdg-open https://raw.githubusercontent.com/elseawhy/xdg-mimic/refs/heads/main/xdg-open
chmod +x ~/.local/bin/xdg-open
```

## 🤝 Contributing
Since this is experimental, feel free to open issues or PRs if you find edge cases with specific file types or desktop environments!
