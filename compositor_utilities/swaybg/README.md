# Swaybg

## Overview

swaybg is a wallpaper-setting utility designed for Wayland compositors (for example, Sway) that implement the `wlr‑layer‑shell` and `wl_output` protocol extensions. Its principal function is to display a background image (or a solid color) on one or more Wayland outputs.

---

## Installation

Swaybg is packaged in many Linux distributions; it may also be compiled from source.
Typical installation steps include:

* Via package manager: e.g., `sudo apt install swaybg` (Debian/Ubuntu) or `sudo pacman ‑S swaybg` (Arch) depending on your system.
* From source: ensure dependencies such as meson, ninja, wayland, cairo, and optionally gdk‑pixbuf are available. Then run `meson build/`, `ninja ‑C build/`, `sudo ninja ‑C build/ install`.

---

## Basic Usage

The general syntax is:

```
swaybg [options...]
```

Key options include:

* `‑i, --image <path>`; specify the image file to use as the background.
* `‑m, --mode <mode>`; specify how the image is scaled or placed. Valid modes: `stretch`, `fill`, `fit`, `center`, `tile`, and a special `solid_color` mode for only a color.
* `‑c, --color <[#]rrggbb>`; specify a background colour (when no image or in solid mode).
* `‑o, --output <name>`; target a specific output (monitor). The special value `*` may be used to refer to all outputs.

**Example**:

```bash
swaybg -i ~/Pictures/wallpaper.jpg -m fill
```

This command sets the image `wallpaper.jpg` as the background on all outputs, using the “fill” mode.

---

## Advanced Considerations

### Per‑output configuration

When multiple monitors are present, you may wish to specify different images or settings for each output. This is done by combining `‑o` with subsequent settings. For instance:

```bash
swaybg -o eDP‑1 -i ~/Pictures/image0.png -m fill \
       -o HDMI‑A‑2 -i ~/Pictures/image1.png -m fill
```

This assigns different wallpapers to the outputs named `eDP‑1` and `HDMI‑A‑2`.

### Session persistence

It is important to ensure that swaybg is launched in a way that persists across session reloads (not just initial start). For the compositor Sway this typically means adding a line in the config file (e.g., `~/.config/sway/config`):

```bash
exec --no‑startup‑id swaybg -i /path/to/image.jpg -m fill
```

This ensures that on each session start (or reload) the background is set.

### Handling existing instances

If another instance of swaybg is already running, simply launching a new one may lead to multiple duplicate background processes or unexpected behaviour.It is recommended you kill the existing instance before launching a new one.

---

## Modes Explained

Below is a summary of the scaling/placement modes:

* **fill**: The image is scaled to fill the output, preserving aspect ratio; parts may be cropped.
* **fit**: The image is scaled to fit within the output, preserving aspect ratio; may leave borders.
* **stretch**: The image is stretched to fill the output entirely; aspect ratio may be lost.
* **center**: The image is placed centered; no scaling by default.
* **tile**: The image is repeated (tiled) across the output.
* **solid_color**: No image is used; the background is only the specified colour.

---

## Limitations and Notes

* swaybg only supports Wayland compositors that implement the appropriate protocols (wlr‑layer‑shell and version‑4 of wl_output).
* When launched from a terminal, if you close the terminal, the background process may terminate; thus running via `exec` in config or by disowning the process is advisable.

---
