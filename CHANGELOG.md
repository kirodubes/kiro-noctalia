# Changelog

## 2026.11.15

### What Changed
- The screen now turns off after 15 minutes idle, like XFCE. noctalia ships its screen-off idle step disabled, so
  the screen never went off. The idle lock is listed too, at 15 minutes, but stays off: turn it on in Settings.

### Technical Details
- `00-kiro.toml`: `[idle.behavior.screen-off]` (timeout 900, `noctalia msg dpms-off` / `noctalia msg dpms-on`,
  enabled) and `[idle.behavior.lock]` (timeout 900, `noctalia msg session lock`, enabled = false). Keys from
  noctalia's `example.toml` and `src/config/schema/config_schema.cpp` (`idle.behavior`).
- The commands run in a shell. The upstream example's `noctalia:dpms-off` form does not exist in noctalia 5.4.1:
  on picard (niri noctalia) the idle step fired at 15 min but the screen stayed on; with `noctalia msg dpms-off`
  the screen went black at 30 s (test timeout). Built first as 26.11-01/26.11-02 with the broken form.
- Reaches new installs only (`/etc/skel`); existing installs get it with `skell`. Same change in kiro-hyprland-noctura (its own copy).

### Files Modified
- `etc/skel/.config/noctalia/00-kiro.toml`

## 2026.10.23

### What Changed
- **The colours follow the wallpaper, terminal included.** The noctalia editions used the fixed Catppuccin palette, so the bar, launcher and Alacritty kept the same colours whatever the wallpaper. noctalia now builds its palette from the wallpaper (Variety hands each new one to noctalia, so Alt+N and Alt+Shift+N recolour), and Alacritty takes the same colours, as on the DMS editions. Existing installs get it with `skell` and a log-in; a theme you pick in noctalia's Settings still wins.

### Technical Details
- `00-kiro.toml`: `[theme] source = "wallpaper"` (was `builtin` / `Catppuccin`) and `[theme.templates] builtin_ids = ["alacritty"]`. Built-in templates are on by default in noctalia but none is selected; the Alacritty template writes `~/.config/alacritty/themes/noctalia.toml` and its own `apply.sh` adds the import to `alacritty.toml` (creates it when missing, edits an existing one). No Kiro helper script needed, unlike the DMS editions. Switching the template off removes the import again (seen after a `skell` back to the old file).
- Key names checked against noctalia v5.3.0 source; `noctalia config validate` passes. Tested on picard (kiro-hyprland-noctalia): the theme file and import appear at once and a Variety wallpaper changes the palette. On riker (kiro-niri-noctalia) the same after installing the package and running `skell`: each Variety wallpaper gave a new palette.

### Files Modified
- `etc/skel/.config/noctalia/00-kiro.toml`

## 2026.10.14

### What Changed
- Ported the shared config to noctalia v5. noctalia-shell v4 (Quickshell) broke on Qt 6.12: noctalia-qs was built against Qt 6.11, so every themed widget got an undefined colour and the bar rendered as empty pills. v4 is unmaintained upstream and [cachyos] now ships v5 under the noctalia-shell name without shell.qml. Native noctalia v5.2.1 from Arch [extra] was live-tested on a real-metal test box and works.

### Technical Details
- v4 JSON (`settings.json`, `colors.json`, `plugins.json`) replaced by one `00-kiro.toml` holding only Kiro's non-default choices: Noto Sans, Catppuccin dark built-in palette, top bar with the same widget layout, auto-hide dock, `polkit_agent = true` (replaces the v4 polkit-agent plugin), `telemetry_enabled = false`, `setup_wizard_enabled = false` (no first-run welcome on the ISO).
- The `00-` prefix makes a user's own `config.toml` load after it; Settings changes go to `~/.local/state/noctalia/settings.toml` and win over both.
- Validated with `noctalia config validate` (v5.2.1). Recipe now `depends=('noctalia')`.
- Dock: `reserve_space = false`. v5 keeps an auto-hidden dock's strip reserved by default, which left a 76 px empty gap under tiled windows (found on a real-metal install).

### Files Modified
- etc/skel/.config/noctalia/00-kiro.toml (new)
- etc/skel/.config/noctalia/settings.json, colors.json, plugins.json (removed)
- README.md, CLAUDE.md

## 2026.10.10

### What Changed
- Source repo moved from `~/KIROTUX/kiro-noctalia` to `~/KIRO/kiro-noctalia` and its recipe to `~/KIRO-PKG-BUILD-APPS/kiro-noctalia`: ATT installs it from nemesis_repo, so it is a Kiro package, not KiroTux-only. Paths and links in the docs follow. The package itself is unchanged.

### Files Modified
- `CLAUDE.md`, `README.md`, `CHANGELOG.md`

## 2026.07.04

### What Changed
- Initial package. Extracted the shared **noctalia-shell** default config
  (`~/.config/noctalia/{settings,colors,plugins}.json`) out of the individual
  `kiro-<wm>-noctalia` editions into this one shared package.
- Reason: noctalia always reads the fixed path `~/.config/noctalia/`, so it
  cannot be namespaced per compositor. Both `kiro-hyprland-noctalia` and
  `kiro-niri-noctalia` shipped identical `/etc/skel/.config/noctalia/*` files,
  causing a pacman file conflict that blocked co-installation. A single owner
  package that both editions depend on resolves it.

### Technical Details
- Ships `etc/skel/.config/noctalia/` (byte-identical to the config the two
  editions previously carried) plus a read-only golden copy at
  `/usr/share/kiro/kiro-noctalia/` for reset-to-defaults.
- No session entry, wrapper, or compositor config — pure shared config.
- Both editions now `depends=('kiro-noctalia')` and no longer ship the
  `noctalia/` folder.

### Files Modified
- etc/skel/.config/noctalia/settings.json (new)
- etc/skel/.config/noctalia/colors.json (new)
- etc/skel/.config/noctalia/plugins.json (new)
- README.md, CLAUDE.md, up.sh, setup.sh, .gitignore, kiro.jpg (new)
