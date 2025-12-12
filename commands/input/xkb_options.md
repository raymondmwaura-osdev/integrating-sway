# xkb_options

## Overview

The `xkb_options` setting in Sway’s `input` configuration or via `swaymsg` is used to specify additional X Keyboard Extension (XKB) options for keyboard devices. These options control modifier behaviors, group switching mechanisms, compose key definitions, and other non‑layout aspects of the keyboard mapping. It applies only to **keyboard** input devices.

+ **Device Types Affected**: keyboard only
+ **Data Format**: list of string options, comma‑separated (e.g., `"grp:alt_shift_toggle,compose:ralt"`)
+ **Default Value**: empty (no extra options applied). If unspecified, the system defaults defined in the XKB data files apply (effectively no additional options beyond the chosen layout)

---

## Valid Values

Sway itself does not enumerate specific `xkb_options`; it accepts arbitrary XKB option strings drawn from the **xkeyboard‑config** project. Valid values are defined by the XKB rules and include (but are not limited to) categories such as:

- **Group switching**: e.g., `grp:alt_shift_toggle`, `grp:win_space_toggle`, `grp:caps_toggle`; let you switch between multiple layouts
- **Modifier remapping**: e.g., `ctrl:nocaps`, `caps:swapescape`, `altwin:swap_lalt_lwin`
- **Compose key definitions**: e.g., `compose:caps`, `compose:ralt`
- **Locking/LED behavior**: e.g., `grp_led:caps`

The full set of valid options is maintained in the XKB database (`/usr/share/X11/xkb/rules/base.lst` and related) and documented in `xkeyboard‑config(7)`

---

## Behavior and Effects

**Immediate Effect**  
When applied via `swaymsg`, `xkb_options` takes effect immediately on the targeted keyboard device. Example runtime change: altering group switcher behavior without restarting Sway. Visual feedback (e.g., active layout name) often updates immediately

**Startup Behavior**  
When placed in the Sway config file under an `input` block, the `xkb_options` are read and applied when Sway starts or when the configuration is reloaded. This ensures persistent option application across sessions

**Runtime Changes via `swaymsg`**  
You may change keys at runtime by addressing the specific keyboard identifier (from `swaymsg -t get_inputs`) or a selector such as `type:keyboard` or `*`. Sway applies the new options immediately. If multiple selectors match a device, the most recently applied command takes effective precedence

**Interactions and Edge Cases**  
- **Composability**: Option lists should be comma‑separated without spaces. Invalid syntax may result in ignored options or unexpected behavior
- **Invalid options**: A mis‑typed XKB option can cause Sway to fail keyboard configuration (no keymap loaded), potentially leaving the session unusable. Always verify valid strings against XKB data
- **Layout vs options**: Some behaviors (e.g., switching between layouts) require both multiple `xkb_layout` values and an appropriate `xkb_options` group switcher
- **Application differences**: Certain Compose behaviors may not be respected uniformly across all Wayland clients if locale settings and Compose maps differ

---

## Examples

### Terminal Example

```bash
# Apply a runtime change: set the right Alt key as a Compose key
swaymsg input "type:keyboard" xkb_options "compose:ralt"
```

```bash
# Enable Alt+Shift group toggle at runtime
swaymsg input <identifier> xkb_options "grp:alt_shift_toggle"
```

### Config File Example

```bash
# Apply extra XKB options in the Sway configuration
input "type:keyboard" {
    xkb_layout "us,ru"
    xkb_options "grp:alt_shift_toggle,ctrl:nocaps"
}
```

---

## Notes

* Values for `xkb_options` originate from the **xkeyboard‑config** project, not Sway itself; check your distribution’s `/usr/share/X11/xkb/rules/base.lst` for the definitive list
* When changing `xkb_options` at runtime, the effects are immediate, but persistent use requires config file placement
* Improper options can break keyboard input entirely; ensure each option is valid and test changes cautiously

---
