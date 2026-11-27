# TODO

Roadmap for `hypr-mocha-dots` and its standalone `fuzzyland` shell backend.

## Prototype Features
- [x] **Clipboard history** — daemon-supervised `wl-paste`/`cliphist` watchers (`[clipboard] watch = true` in `config/fuzzyland/config.toml`) + Waybar center pill icon + Fuzzel history picker (`Super + Shift + V`, with delete & clear confirmation)
- [x] **Screenshots** — region / window / monitor / all-monitors capture (`grim` + `slurp`), copy to clipboard by default + optional save toggle + thumbnail notification (`Print` / `Super + Print`)
- [x] **Idle & auto-lock (`hypridle`)** — dim screen via `fuzzyland backlight internal dim`/`restore` (2.5 min), lock via `hyprlock` (5 min), DPMS off (5.5 min), and suspend (30 min) via `config/hypr/hypridle.conf` + `hypridle.service`, with sleep lock inhibition, `Super + L` lock bind, and Waybar's `idle_inhibitor` wired up
- [x] **Notification daemon config** *(optional)* — minimal toast-only `dunst` config (`config/dunst/dunstrc`) with decoupled Catppuccin Mocha Lavender theme drop-in (`config/dunst/dunstrc.d/50-catppuccin-mocha.conf`), Fuzzel action menu, and hover-to-pause

## Architecture & Prod Rewrite (`fuzzyland`)
- [x] **Rewrite scripts in a compiled language** — consolidated the 11 Python/Bash shell scripts into a single Rust binary (`fuzzyland`) with subcommands and pruned legacy scripts from `bin/`
- [x] **Reactive D-Bus integration**
  - [x] Stream `custom/battery` directly from `org.freedesktop.UPower` + `org.freedesktop.UPower.PowerProfiles` signals (eliminated 5s polling & `RTMIN+8`)
  - [x] Query & control Bluetooth via `org.bluez` (`GetManagedObjects` + embedded `Agent1`) + `/dev/rfkill`
  - [x] Control networking natively via `org.freedesktop.NetworkManager` over D-Bus (including `fuzzyland network --toggle-wifi`)
  - [x] Native MPRIS media controls (`fuzzyland media`) across Waybar, Hyprland, and Hyprlock (dropped `playerctl`)
  - [x] Native audio volume/mute (`fuzzyland audio output/input --volume/--toggle-mute`) and built-in display backlight (`fuzzyland backlight internal adjust/dim/restore`) across Waybar, Hyprland, and Hypridle (dropped `wpctl` and `brightnessctl`)
- [x] **Hyprland IPC socket** — direct `.socket.sock` / `.socket2.sock` client for monitor/workspace/window queries and external backlight hotplug events
- [x] **Single background daemon** — `fuzzyland.service` holds warm D-Bus/Hyprland/DDC state (~2–3 MB PSS), supervises clipboard history watchers (`[clipboard] watch = true`), and provides centralized menu toggle coordination with transparent standalone fallback

## Theme Decoupling & Packaging
- [x] **Remove hardcoded colors from shell backend** — `fuzzyland` defaults to inherited foreground + Pango alpha dimming and neutral `slurp` colors; Catppuccin Mocha colors live in optional `config/fuzzyland/config.toml`
- [x] **Separate theme files from shell core** — `fuzzyland` is fully theme-agnostic; Waybar CSS, Fuzzel config, and Dunst drop-in theme remain independently swappable
- [ ] **Bootstrap/install script** *(optional)* — automate symlinking `config/*`, `bin/*`, and enabling user systemd units on a fresh machine

## Non-Goals (Intentionally Excluded)
- **No OSD popups** — Waybar already surfaces volume/brightness/mute state cleanly
- **No notification center** — toast notifications only
- **No per-app audio mixer** — kept simple with output/input switching + EasyEffects presets
