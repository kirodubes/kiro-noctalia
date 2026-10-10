# Changelog

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
