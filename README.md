# whatpulse-aur

Unofficial, fixed AUR PKGBUILD for [WhatPulse](https://www.whatpulse.org/) — the cross-platform desktop application that tracks your keystrokes, mouse clicks, bandwidth usage, uptime, and other delightfully nerdy stats.

This repo exists because the official [AUR package](https://aur.archlinux.org/packages/whatpulse) is outdated or broken due to upstream changes (e.g., the switch to `whatpulse-linux-latest_amd64.AppImage` with no versioned URLs).
## Installation

Clone the repo and build it manually with `makepkg`:

```bash
git clone https://github.com/otsegolo/whatpulse-aur.git
cd whatpulse-aur
makepkg -si
```
