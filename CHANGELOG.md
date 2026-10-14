# Changelog

## 2026.10.14

### What Changed
- Ported the shared config to noctalia v5. noctalia-shell v4 (Quickshell) broke on Qt 6.12: noctalia-qs was built against Qt 6.11, so every themed widget got an undefined colour and the bar rendered as empty pills. v4 is unmaintained upstream and [cachyos] now ships v5 under the noctalia-shell name without shell.qml. Native noctalia v5.2.1 from Arch [extra] was live-tested on a real-metal test box and works.

### Technical Details
- v4 JSON (`settings.json`, `colors.json`, `plugins.json`) replaced by one `00-kiro.toml` holding only Kiro's non-default choices: Noto Sans, Catppuccin dark built-in palette, top bar with the same widget layout, auto-hide dock, `polkit_agent = true` (replaces the v4 polkit-agent plugin), `telemetry_enabled = false`, `setup_wizard_enabled = false` (no first-run welcome on the ISO).
- The `00-` prefix makes a user's own `config.toml` load after it; Settings changes go to `~/.local/state/noctalia/settings.toml` and win over both.
- Validated with `noctalia config validate` (v5.2.1). Recipe now `depends=('noctalia')`.

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
