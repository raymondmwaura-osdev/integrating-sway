# `default_border`

## Overview

The `default_border` command in **Sway** is used to define the default border style and thickness for **new tiled windows**. It belongs primarily to the **window management** domain, since it determines how windows are decorated when they are first spawned. The setting influences only newly created windows; existing windows at the time of configuration reload remain unaffected.

---

## Syntax

```
default_border normal|none|pixel [<n>]
```

Here:

- `normal`, `none`, `pixel` select the border style.
- `<n>` is an optional integer (number of pixels) specifying the thickness of the border when the style supports it.

---

## Parameters / Arguments

+ `normal`: This style gives a **title bar** plus a border. The optional `<n>` defines how many pixels thick the border is.
+ `none`: No border, no title bar. If `<n>` is omitted, the thickness is implicitly zero.
+ `pixel`: Only a border (no title bar), with thickness `<n>` pixels. If `<n>` is omitted, a default is assumed.
+ `<n>` (integer): Sets the thickness of the border in pixels; only meaningful when the style is `normal` or `pixel`.

---

## Behavior and Effects

- **Application to New Windows Only**: `default_border` only affects tiled windows created *after* the setting is read (for example, after reloading the config). Existing windows keep their previous border settings.
- **Corner Cases / Special Conditions**:
  - For **stacked** or **tabbed** layouts, Sway may not respect the `pixel` or `none` styles entirely. Titlebars can persist in those layouts.
  - Because `none` means no border, resizing by mouse may be more challenging if you rely on border regions for drag handles.

---

## Usage Examples

**Terminal Example (via `swaymsg`)**

```bash
swaymsg "default_border pixel 2"
```

This sets the default border style for new tiled windows to a 2-pixel border, without titlebar, for any subsequently opened window.

**Config File Examples (in `~/.config/sway/config`)**

1. **No border, no title bar**:

   ```bash
   default_border none
   ```

   This means tiled windows launched after this setting are borderless and titleless.

2. **Minimal pixel border (without title bar)**:

   ```bash
   default_border pixel 1
   ```

   * Tiled windows: 1-pixel border, no title bar.

---

## Related Commands

* `default_floating_border normal|none|pixel [<n>]`: Sets the default border style for *floating windows*.
* `border none|normal|csd|pixel [<n>]`: Runtime command to change the border style of the **currently focused** window.
* `border toggle`: Cycles through the border styles for the focused window.
* `hide_edge_borders`: Controls the hiding of borders adjacent to screen edges. Often used in conjunction with default_border to tighten UI.
* `smart_borders`: Dynamically shows or hides borders based on number of children in workspace; often used with default_border.

---
