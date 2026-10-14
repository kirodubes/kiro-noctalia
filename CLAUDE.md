# kiro-noctalia — Claude project instructions

## Overview
Shared **noctalia** (v5, native) default config for the KIROTUX Wayland line. Public,
open-core, shipped via `nemesis_repo`. Owns exactly `~/.config/noctalia/`
(`00-kiro.toml`) — nothing compositor-specific.

## Why this package exists
noctalia always reads a single fixed path, `~/.config/noctalia/`,
independent of the compositor that spawned it. So — unlike the compositor
config, which each edition namespaces to `~/.config/kiro-<wm>-noctalia/` — the
noctalia config cannot be namespaced. When two editions
(`kiro-hyprland-noctalia`, `kiro-niri-noctalia`) each shipped their own
`/etc/skel/.config/noctalia/` config, pacman reported a file conflict and
they could not co-install. Extracting the shared folder into this single
package that both editions `depends=()` on removes the conflict.

## What lives here vs in an edition
- **Here:** `etc/skel/.config/noctalia/00-kiro.toml` — only the values Kiro
  changes from noctalia v5's built-in defaults (palette, font, bar widgets,
  dock, `polkit_agent`, `telemetry_enabled`, `setup_wizard_enabled`).
- **In each `kiro-<wm>-noctalia` edition:** the compositor config
  (`~/.config/kiro-<wm>-noctalia/`), the `.desktop` session, the launcher
  wrapper, any per-WM hooks. Not this package's business.

## Rules / gotchas
- The config is **compositor-agnostic** — keep it that way. Nothing here may
  assume Hyprland, niri or any specific WM; per-WM behaviour belongs in the
  edition package. If a setting only makes sense for one compositor, it does
  not belong in this shared file.
- `noctalia` (v5) comes from Arch **`[extra]`** — do **not** repackage the shell
  into `nemesis_repo`. This package is config only. Never depend on
  `noctalia-shell`: [cachyos] ships v5 under that name without `shell.qml`, and
  the real v4 broke on Qt 6.12 (2026-10-14).
- When bumping the config, remember every noctalia edition inherits it at once.
  Run `noctalia config validate <file>` — it flags unknown keys (not unknown
  widget ids; check those against docs.noctalia.dev). The `00-` prefix keeps a
  user's own `config.toml` loading after (and overriding) the Kiro defaults.

## Build / delivery
- Config source-of-truth; delivered as the `kiro-noctalia` package via
  `~/KIRO-PKG-BUILD-APPS/kiro-noctalia/build.sh` (public recipe → `~/EDU/nemesis_repo/`).
- Flow: `flow-kiro-noctalia`. Build this **before** the editions that depend on
  it. See [KIROTUX/CLAUDE.md](../../KIROTUX/CLAUDE.md) for the full KIROTUX delivery architecture.
