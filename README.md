# hp-gnome-theme

A dark GNOME desktop theme in the HP brand palette: Electric Blue accents on neutral dark greys. It covers the terminal, GTK apps, the top bar, the dock, the lock screen, the login screen and the boot splash.

> **Unofficial.** This project is not affiliated with, endorsed by, or supported by HP. "HP" and the HP logo are trademarks of HP Inc. The theme itself contains colour values only; no logos, fonts or wallpapers are bundled or installed.

<!-- ![Desktop](docs/screenshots/desktop.png) — add screenshots, see docs/screenshots/README.md -->

## What's included

| Component | What it does | Works on |
|---|---|---|
| `terminal` | [Ghostty](https://ghostty.org) theme, tmux colours, [btop](https://github.com/aristocratos/btop) theme | Anywhere |
| `gtk` | Accent and surface colours for GTK4/libadwaita and GTK3 apps | Any GNOME (GTK ≥ 4.20) |
| `shell` | GNOME Shell extension: dark top bar with a blue Activities pill, popups, lock screen | GNOME Shell 50 |
| `desktop` | Dark mode, blue accent, brand font, optional wallpaper | Any GNOME |
| `dock` | Dark dock with blue running-app dots | Ubuntu Dock / Dash to Dock |
| `--gdm` | Login screen: same colours, font and wallpaper | Ubuntu / Debian with Yaru |
| `--plymouth` | Boot splash: wallpaper or dark background, blue progress bar | Any Plymouth distro; tested on Ubuntu |

Tested on **Ubuntu 26.04 LTS** (GNOME 50, Yaru). The core components should work on other GNOME distros but haven't been tested there.

## Install

```sh
git clone https://github.com/garethsprice/hp-gnome-theme
cd hp-gnome-theme
./install.sh                                   # user-level components, no sudo
./install.sh --wallpaper ~/Pictures/hp.jpg    # also set a wallpaper
./install.sh --gdm --plymouth --wallpaper ~/Pictures/hp.jpg   # everything (asks for sudo)
```

Log out and back in once so GNOME loads the shell extension. After that, running `./install.sh` again applies updates without logging out.

### Options

```
--only LIST          Comma-separated subset: terminal,gtk,shell,desktop,dock
--wallpaper PATH     Image for the desktop, login screen and boot splash
--font NAME          Font family (default: HP Forma DJR Office, then Montserrat)
--font-size N        Interface font size (default: 10)
--install-deps       apt install fonts-montserrat if no brand font is found
--gdm                Also theme the login screen (uses sudo)
--plymouth           Also theme the boot splash (uses sudo, rebuilds the initramfs)
--early-kms MODULE   With --plymouth: load a GPU driver early (see below)
```

### Fonts

HP's guidelines use **HP Forma DJR Office**: Medium weight for headlines and Regular weight for all other copy. It is a proprietary typeface and is not included. If you have a licensed copy installed, the installer uses it. Otherwise it falls back to **Forma DJT**, and then to **Montserrat**.

- **Forma DJT** is a free typeface you can download online (search "Forma DJT font" — it is distributed from the foundry's own site and a number of font repositories). Install it with your normal font manager (e.g. `sudo fc-cache` after dropping the `.ttf`/`.otf` into `~/.local/share/fonts` or `/usr/share/fonts`) and the installer will pick it up automatically. **Check the licence before you use it:** free-to-download does not always mean free-to-embed in a desktop theme, so read the foundry's licence terms and make sure personal/desktop use is permitted before relying on it.
- **Montserrat** is free (SIL Open Font License), in Ubuntu's repos as `fonts-montserrat`, and a close geometric-sans substitute. Use `--install-deps` to have the installer fetch it when neither brand font is present.

Monospace and terminal fonts aren't changed.

### Wallpaper

None is bundled. Pass any image with `--wallpaper`. Without one, the login screen and boot splash use a plain dark grey background.

- **Official:** HP publishes branded imagery on its [Brand Central](https://www.hp.com/us-en/hp-information/brandcentral.html) page. Check the terms for any image you download.

### Boot splash doesn't appear, or appears late?

On some multi-GPU machines, the firmware treats a card with no monitor attached as the boot display. The splash then has nothing to draw on until the real GPU driver loads, which can be the last few seconds of boot. Adding the driver to the initramfs fixes it:

```sh
./install.sh --plymouth --early-kms amdgpu   # or i915, nouveau, …
```

This makes the initramfs bigger (by about 35 MB for amdgpu). The previous initramfs is kept as `/boot/initrd.img-<kernel>.hp-gnome-theme.bak`. If the machine won't boot, press `e` in GRUB, add that suffix to the `initrd` line, and boot with Ctrl-X.

## Uninstall

```sh
./uninstall.sh
```

This restores every GNOME setting the installer changed and removes the files and config blocks it added. If the login screen or boot splash were installed, it reverts those too (asks for sudo).

## How it works

- **Nothing is overwritten.** The installer adds clearly marked blocks (`>>> hp-gnome-theme >>>`) to `gtk.css`, `~/.tmux.conf` and the Ghostty config, and `uninstall.sh` removes only those blocks. It saves the original value of each GNOME setting to `~/.local/state/hp-gnome-theme/`.
- **The top bar** is styled by a tiny extension that only contains CSS, because GNOME Shell has no user stylesheet. It also runs on the lock screen, where extensions are off by default.
- **The login screen** theme is built on your machine from the installed Yaru theme with `hp-gdm.css` added on top, then selected through `update-alternatives`. After a major GNOME or Yaru upgrade, re-run `./install.sh --gdm` to rebuild it.
- **The boot splash** is a Plymouth `two-step` theme. The installer adds the font (and an optional GPU driver) to the initramfs, using either dracut or initramfs-tools, whichever the system uses.
- **Desktop icons fix:** on Ubuntu, the desktop icons extension (DING) draws a transparent GTK3 window over the wallpaper. `gtk-3.0.css` keeps that window transparent so the wallpaper still shows.

## Releases

```sh
make release   # builds dist/hp-gnome-theme-<version>.tar.gz and dist/hp-gnome-theme-plymouth-<version>.tar.gz
make lint      # shellcheck
```

The version comes from `VERSION`. The full archive is `git archive` of `HEAD` when the tree is clean, otherwise the working tree. The Plymouth archive is a self-contained boot-splash theme (generic font, dark background, stock spinner images under GPL-2+) for anyone who only wants the splash. It needs `plymouth-theme-spinner` installed on the machine that builds it.

## Palette

See [palette.md](palette.md) for the brand colours, the derived ones, and contrast notes.

## Licence

MIT, see [LICENSE](LICENSE). The login screen theme is built from Yaru on your machine, and no Yaru files are included here; see [NOTICE](NOTICE).
