# Palette

Brand colours come from HP's published guidelines ([hp.com/brandcentral](https://www.hp.com/us-en/hp-information/brandcentral/our-visual-identity.html)). HP has no brand greens, yellows or cyans, and the brand blue is too saturated for some text contexts on a dark background, so the terminal palette adds derived colours. They're marked *derived* below.

## Brand

| Name | Hex | Used for |
|---|---|---|
| Electric Blue | `#0096D6` | Accent everywhere: selections, toggles, tmux status bar, Activities pill, dock dots |
| Light Blue | `#4DB8E8` | Blue text and links on dark backgrounds (6.2:1 on `#1A1E22`; Electric Blue is 4.2:1, below WCAG AA for text) |
| Dark Blue | `#006FA3` | Pressed and destructive states, borders |
| Deep Blue | `#004D6E` | Unfocused selection, tmux message and copy-mode bars, btop selection |
| Pale Blue | `#B3D9F0` | Bright yellow in the terminal palette |
| Dark Gray | `#3D4A52` | Terminal black, pane borders, progress-bar track |
| Mid Gray | `#9AAAB5` | Terminal white (ANSI 7) |
| Pale Gray | `#E2E8EC` | Main text |
| Pale Blue 2 | `#A0D4F0` | Terminal bright blue |
| Pale Cyan | `#A0D4E0` | Terminal bright magenta, top of btop upload graph |
| Deep Teal | `#1A5A6E` | Bottom of btop upload graph |
| White | `#FFFFFF` | Text on Electric Blue (4.2:1) |

## Neutral surfaces (derived)

The brand's blue is saturated; the surfaces use neutral blue-tinted greys.

| Hex | Used for |
|---|---|
| `#14181C` | GTK content views |
| `#1A1E22` | Window background, terminal, top bar, dock, login screen |
| `#1F2428` | Sidebars |
| `#242A30` | Header bars, popovers, shell menus |
| `#343A42` | Notifications |

## Terminal colours (derived)

| ANSI | Normal | Bright |
|---|---|---|
| Green | `#4DB88A` | `#8AD8B8` |
| Yellow | `#E8C84D` | `#B3D9F0` |
| Blue | `#4D8AD6` | `#A0D4F0` |
| Magenta | `#8A5AB4` | `#A0D4E0` |
| Cyan | `#4DB4C8` | `#8ADBE6` |
| Bright black | `#8A97A0` | |

btop load graphs (CPU, temperature, used memory, per-process CPU) run green `#4DB88A` → yellow `#E8C84D` → Electric Blue, so blue only ever means high load.

## Type

The guidelines use HP Forma DJR Office Medium for headlines and Regular for body text. Forma DJR (free, downloadable online — check its licence; install the "Forma DJR Text" family) and Montserrat are the fallbacks. See the README's Fonts section.
