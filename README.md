# kiro-noctalia

Shared **noctalia-shell** default configuration for the KIROTUX Wayland line.

This package owns the compositor-agnostic noctalia config that lives at
`~/.config/noctalia/` — `settings.json`, `colors.json` and `plugins.json`.
noctalia always reads that single fixed path regardless of which compositor
launched it, so the config cannot be namespaced per edition the way the
compositor configs are. Shipping it from one shared package instead of from
every `kiro-<wm>-noctalia` edition removes the `/etc/skel/.config/noctalia/*`
file conflict between the editions and lets them co-install.

## Who depends on this

Every noctalia-shell edition of Kiro:

- `kiro-hyprland-noctalia` (Hyprland + noctalia-shell)
- `kiro-niri-noctalia` (niri + noctalia-shell)

Each edition `depends=('kiro-noctalia')` and no longer ships its own
`noctalia/` folder. The compositor config (`~/.config/kiro-<wm>-noctalia/`),
session entry and wrapper stay in the edition package.

## What it does NOT own

The noctalia shell itself (`noctalia-shell` / `noctalia-qs`) comes from
chaotic-aur / cachyos — never packaged by Kiro. Base GTK/cursor look ships
via `kiro-wayland-dotfiles`. This package is purely the default `~/.config/noctalia/`.

## Delivery

Config source-of-truth; delivered as the `kiro-noctalia` package via
`~/KIRO-PKG-BUILD-APPS/kiro-noctalia/build.sh` (public recipe → `~/EDU/nemesis_repo/`).
