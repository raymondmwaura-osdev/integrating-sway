# `input`

## Overview

The `input` command in Sway is a **device configuration directive** used to define properties for input hardware such as keyboards, touchpads, mice, and other libinput‑recognised devices. Its primary function is to control **hardware‑level behavior** including key repeat settings, keyboard layout, pointer acceleration, scroll behaviour, and other device‑specific options. It is used within the **Sway configuration file** and is categorised under **system input configuration**, not the primary window management command class.

---

## Syntax

```
input <identifier> <setting> [<value>]
```

Where:

- `<identifier>` is either:
  - A **device identifier** string (e.g., `"1:1:AT_Translated_Set_2_keyboard"`),
  - A **type selector** (e.g., `type:keyboard`, `type:touchpad`),
  - The wildcard `*` to match **all input devices**.

---

## Parameters / Arguments

+ `identifier`: A device selector that specifies which input device(s) the setting applies to. This may be a literal hardware identifier, a type alias, or the wildcard `*`.
+ `setting`: The configuration directive to apply (e.g., `xkb_layout`, `tap`, `pointer_accel`).
+ `value`: The value assigned to the setting. This may be a string, numeric value, or keyword depending on the setting.

Common settings include:

+ `xkb_layout`: Sets the keyboard layout (e.g., `us`, `de`). Multiple layouts can be comma‑separated.
+ `xkb_variant`: Sets a variant for a selected layout (e.g., `colemak`).
+ `repeat_delay`: Configures the **delay before a held key begins repeating** (ms).
+ `repeat_rate`: Sets the **character repeat rate** when a key is held (characters per second).
+ `tap enabled|disabled`: Enables or disables **tap‑to‑click** for touchpads.
+ `pointer_accel`: Adjusts pointer acceleration for mice or touchpads (range between -1 and 1).
+ `scroll_method`: Defines the scroll mechanism (`none`, `two_finger`, `edge`, `on_button_down`).
+ `scroll_button`: Specifies the mouse button used to initiate scroll method on button down.

---

## Behavior and Effects

When executed in the **Sway configuration file**, the `input` command configures input devices at **startup**. Devices matching the `<identifier>` will acquire the specified settings before the window manager fully applies desktop behaviour. Selector precedence in config files is:

1. **Literal identifier** (most specific)
2. **Type selector**
3. **Wildcard `*`** (least specific)

However, when `input` is **executed at runtime** (e.g., via `swaymsg`), settings are applied to all matching devices dynamically, and more general selectors can **override previously applied settings**.

Examples of special behavior includes:

+ Using `type:keyboard` applies settings to all keyboard devices without requiring exact identifiers.
+ The wildcard `*` can be used as a fallback if device identifiers change or are unknown.
+ Settings like pointer acceleration are only meaningful for pointer‑type devices; applying them to non‑pointer devices has no effect.

Edge cases:

+ If a more specific configuration exists (e.g., per‑device entry) but a later wildcard configuration is applied, the wildcard can override unless ordering is controlled.
+ Some settings (e.g., `xkb_capslock enabled|disabled`) are only supported in the config file and not valid via `swaymsg` at runtime.

---

## Usage Examples

### Terminal Example

1. **Switch keyboard layout of all keyboards to US** (immediate runtime change):

```bash
swaymsg input type:keyboard xkb_layout us
```

2. **Enable tap‑to‑click for all devices** (runtime behavioural change):

```bash
swaymsg input * tap enabled
```

### Config File Example

1. **Set German keyboard layout and toggle option**:

```bash
input "type:keyboard" {
    xkb_layout de
    xkb_options grp:alt_shift_toggle
}
```

2. **Configure touchpad behaviour (tap and natural scroll)**:

```bash
input "type:touchpad" {
    tap enabled
    natural_scroll enabled
    pointer_accel 0.3
    scroll_method two_finger
}
```

---

## Related Commands

* `swaymsg -t get_inputs`: Lists all available input devices and their identifiers.
* `seat <name> ...`: Configures input seat policies and grouping.
* `bindsym`: Binds key combinations to execute commands including `swaymsg input …`.
* `output`: Configures display output behaviour (often used in conjunction with input for multi‑display setups).

---
