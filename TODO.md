# TODO

Roadmap for prototyping remaining shell features and evolving this setup into a standalone, theme-agnostic Waybar + Fuzzel shell backend.

## Prototype Features
- [x] **Clipboard history** — background watchers (`wl-paste --type text/image --watch cliphist store`) + Waybar center pill icon + Fuzzel history picker (`Super + Shift + V`, with delete & clear confirmation)
- [x] **Screenshots** — region / window / monitor / all-monitors capture (`grim` + `slurp`), copy to clipboard by default + optional save toggle + thumbnail notification via `bin/screenshot` and Fuzzel menu (`Print` / `Super + Print`)
- [x] **Idle & auto-lock (`hypridle`)** — dim screen (2.5 min), lock via `hyprlock` (5 min), DPMS off (5.5 min), and suspend (30 min) via `config/hypr/hypridle.conf` + `hypridle.service`, with sleep lock inhibition, `Super + L` lock bind, and Waybar's `idle_inhibitor` wired up
- [x] **Notification daemon config** *(optional)* — minimal toast-only `dunst` config (`config/dunst/dunstrc`) with decoupled Catppuccin Mocha Lavender theme drop-in (`config/dunst/dunstrc.d/50-catppuccin-mocha.conf`), Fuzzel action menu, and hover-to-pause

## Architecture & Prod Rewrite
- [ ] **Rewrite scripts in a compiled language** — consolidate `bin/*` Python/Bash prototypes into a single fast binary with subcommands (zero interpreter startup delay on menu clicks)
- [ ] **Reactive D-Bus integration**
  - [ ] Stream `custom/battery` directly from `org.freedesktop.UPower` + `org.freedesktop.UPower.PowerProfiles` signals (eliminate 5s polling & `RTMIN+8`)
  - [ ] Query & control Bluetooth via `org.bluez` (`GetManagedObjects`) instead of spawning/parsing `bluetoothctl`
  - [ ] *(Optional)* Talk to `org.freedesktop.NetworkManager` over D-Bus instead of spawning `nmcli`
- [ ] **Hyprland IPC socket** — subscribe directly to monitor/workspace events instead of spawning `hyprctl` on every click or hook
- [ ] **Single background daemon** *(optional)* — hold persistent D-Bus/Hyprland state in memory so Fuzzel menus open instantaneously

## Theme Decoupling & Packaging
- [ ] **Remove hardcoded colors from scripts & tooltips** — replace Catppuccin hex codes in `waybar-battery-status`, `waybar-external-backlight`, `config.jsonc` tooltips, and `desktop-context-menu` with inherited colors/opacity or a small palette config
- [ ] **Separate theme files from shell core** — make Waybar CSS and Fuzzel theme fully swappable/optional so users can bring their own theme while keeping all shell functionality

## Non-Goals (Intentionally Excluded)
- **No OSD popups** — Waybar already surfaces volume/brightness/mute state cleanly
- **No notification center** — toast notifications only
- **No per-app audio mixer** — kept simple with output/input switching + EasyEffects presets
