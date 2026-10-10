# kiro-noctalia — Claude project instructions

## Overview
Shared **noctalia-shell** default config for the KIROTUX Wayland line. Public,
open-core, shipped via `nemesis_repo`. Owns exactly `~/.config/noctalia/`
(`settings.json`, `colors.json`, `plugins.json`) — nothing compositor-specific.

## Why this package exists
noctalia-shell always reads a single fixed path, `~/.config/noctalia/`,
independent of the compositor that spawned it. So — unlike the compositor
config, which each edition namespaces to `~/.config/kiro-<wm>-noctalia/` — the
noctalia config cannot be namespaced. When two editions
(`kiro-hyprland-noctalia`, `kiro-niri-noctalia`) each shipped their own
`/etc/skel/.config/noctalia/settings.json`, pacman reported a file conflict and
they could not co-install. Extracting the shared folder into this single
package that both editions `depends=()` on removes the conflict.

## What lives here vs in an edition
- **Here:** `etc/skel/.config/noctalia/{settings,colors,plugins}.json` — the
  bar/launcher/lock/notifications/wallpaper/control-center/session-menu config,
  and the noctalia-plugins source (incl. the polkit-agent plugin).
- **In each `kiro-<wm>-noctalia` edition:** the compositor config
  (`~/.config/kiro-<wm>-noctalia/`), the `.desktop` session, the launcher
  wrapper, any per-WM hooks. Not this package's business.

## Rules / gotchas
- The config is **compositor-agnostic** — keep it that way. Nothing here may
  assume Hyprland, niri or any specific WM; per-WM behaviour belongs in the
  edition package. If a setting only makes sense for one compositor, it does
  not belong in this shared file.
- `noctalia-shell` / `noctalia-qs` come from **chaotic-aur / cachyos** — do
  **not** repackage the shell into `nemesis_repo`. This package is config only.
- When bumping the config, remember every noctalia edition inherits it at once.
  `settingsVersion` in `settings.json` must match what the shipped
  `noctalia-shell` expects.

## Build / delivery
- Config source-of-truth; delivered as the `kiro-noctalia` package via
  `~/KIRO-PKG-BUILD-APPS/kiro-noctalia/build.sh` (public recipe → `~/EDU/nemesis_repo/`).
- Flow: `flow-kiro-noctalia`. Build this **before** the editions that depend on
  it. See [KIROTUX/CLAUDE.md](../../KIROTUX/CLAUDE.md) for the full KIROTUX delivery architecture.
