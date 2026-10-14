# kiro-noctalia

Shared **noctalia** (v5, native) default configuration for the KIROTUX Wayland line.

This package owns the compositor-agnostic noctalia config that lives at
`~/.config/noctalia/` — a single `00-kiro.toml` holding only the values Kiro
changes from noctalia's built-in defaults (palette, font, bar layout, dock,
built-in polkit agent, telemetry off, first-run wizard off).
noctalia always reads that single fixed path regardless of which compositor
launched it, so the config cannot be namespaced per edition the way the
compositor configs are. Shipping it from one shared package instead of from
every `kiro-<wm>-noctalia` edition removes the `/etc/skel/.config/noctalia/*`
file conflict between the editions and lets them co-install.

## Who depends on this

Every noctalia edition of Kiro:

- `kiro-hyprland-noctalia` (Hyprland + noctalia)
- `kiro-niri-noctalia` (niri + noctalia)

Each edition `depends=('kiro-noctalia')` and no longer ships its own
`noctalia/` folder. The compositor config (`~/.config/kiro-<wm>-noctalia/`),
session entry and wrapper stay in the edition package.

## What it does NOT own

The noctalia shell itself (`noctalia`, v5) comes from Arch `[extra]` — never
packaged by Kiro. The old v4 Quickshell packages (`noctalia-shell` /
`noctalia-qs`) are no longer used: v4 broke on Qt 6.12 and is unmaintained. Base GTK/cursor look ships
via `kiro-wayland-dotfiles`. This package is purely the default `~/.config/noctalia/`.

## Delivery

Config source-of-truth; delivered as the `kiro-noctalia` package via
`~/KIRO-PKG-BUILD-APPS/kiro-noctalia/build.sh` (public recipe → `~/EDU/nemesis_repo/`).
