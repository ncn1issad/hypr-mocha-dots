<div align="center">

# hypr-mocha-dots

**Catppuccin Mocha (Lavender) dotfiles for Hyprland**

![Hyprland](https://img.shields.io/badge/Hyprland-Lua-89b4fa?style=for-the-badge&logo=hyprland&logoColor=1e1e2e)
![Catppuccin Mocha](https://img.shields.io/badge/Catppuccin-Mocha-b4befe?style=for-the-badge&logo=catppuccin&logoColor=1e1e2e)
![JetBrains Mono](https://img.shields.io/badge/Font-JetBrains%20Mono-cba6f7?style=for-the-badge&logo=jetbrains&logoColor=1e1e2e)

</div>

---

## Stack

| Component | Tool | Notes |
| --- | --- | --- |
| **WM** | Hyprland (`hyprland.lua`) | Lua config with optional gitignored `local.lua` overrides |
| **Bar** | Waybar | MPRIS, Bluetooth, Network, split audio, custom battery & power |
| **Launcher & Menus** | Fuzzel | Interactive menus for Wi-Fi, Bluetooth, audio, clipboard history, power profiles & session |
| **Terminal** | Kitty | Catppuccin Mocha + JetBrains Mono |
| **Lock / Power** | Hyprlock · wlogout | Wallpaper-synced lockscreen with media controls, custom lavender icons |
| **Wallpaper** | `awww` + systemd user timer | Rotates [`orangci/walls-catppuccin-mocha`](https://github.com/orangci/walls-catppuccin-mocha) every 15 min, with Fuzzel desktop right-click menu to skip or remove |
| **System** | btop · UWSM · Fontconfig | Mocha theme, Qt/cursor env, JetBrains Mono defaults |

## Structure

```text
.
├── bin/        # Scripts linked into ~/.local/bin (waybar menus, wallpaper rotation, hyprlock media)
└── config/     # Configs linked into ~/.config (hypr, waybar, fuzzel, kitty, wlogout, btop, systemd, uwsm, fontconfig)
```

## Quick Setup

1. Clone the repo and symlink `config/*` into `~/.config/` and `bin/*` into `~/.local/bin/`.
2. Create machine-specific monitor/input overrides (gitignored):

```bash
cp config/hypr/local.example.lua config/hypr/local.lua
```

3. *(Optional)* Clone wallpapers from [`orangci/walls-catppuccin-mocha`](https://github.com/orangci/walls-catppuccin-mocha) and enable automatic rotation:

```bash
git clone https://github.com/orangci/walls-catppuccin-mocha "$HOME/Pictures/Wallpapers/Catppuccin Mocha"
systemctl --user enable --now catppuccin-wallpaper.timer
```

## Keybinds

| Bind | Action |
| --- | --- |
| `Super + Return` | Terminal (`kitty`) |
| `Super + D` | App launcher (`fuzzel`) |
| `Super + E` | File manager (`dolphin`) |
| `Super + Q` | Close window |
| `Super + V` | Toggle floating |
| `Super + Shift + V` | Clipboard history (`cliphist` + `fuzzel`, also in Waybar center pill — `Shift+Del` delete, `Ctrl+Shift+Del` clear) |
| `Super + Arrows` | Focus window (`+ Shift` to move) |
| `Super + 1–0` / `PgUp` `PgDn` | Switch workspace (`+ Shift` to move window) |
| `Super + S` | Toggle scratchpad (`+ Shift` to send window) |
| `Print` | Screenshot region to clipboard (`+ Super` for Fuzzel screenshot menu) |
| `Super + M` | Exit session |
