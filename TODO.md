# TODO

Roadmap for prototyping remaining shell features and evolving this setup into a standalone, theme-agnostic Waybar + Fuzzel shell backend.

## Prototype Features
- [x] **Clipboard history** — background watcher (`bin/clipboard-history watch` via `wl-paste --watch` + SQLite) + Fuzzel history picker (`Super + Shift + V`, with pin, delete, image previews, and clear confirmation)
- [x] **Screenshots** — region / window / monitor / all-monitors capture (`grim` + `slurp`), copy to clipboard by default + optional save toggle + thumbnail notification via `bin/screenshot` and Fuzzel menu (`Print` / `Super + Print`)
- [ ] **Idle & auto-lock (`hypridle`)** — dim screen, lock via `hyprlock`, DPMS off, and suspend timeouts wired up so Waybar's `idle_inhibitor` has an idle daemon to inhibit
- [ ] **Polkit auth agent** — autostart `hyprpolkitagent` for GUI privilege prompts
- [ ] **Notification daemon config** *(optional)* — minimal toast-only daemon config (e.g. `mako`) to match the theme without a notification center

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
