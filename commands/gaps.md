# `gaps`

## Overview

The `gaps` command in Sway is used for **layout manipulation**; specifically for controlling the amount of spacing (“gaps”) between windows (containers) and between windows and the screen/workspace edges. It allows you to define inner gaps (space between windows) and outer gaps (space between windows and the workspace boundary).

---

## Syntax

```sway
gaps inner|outer|horizontal|vertical|top|right|bottom|left <amount>
```

At runtime (with `swaymsg`) one may also use:

```sway
gaps inner|outer|horizontal|vertical|top|right|bottom|left all|current set|plus|minus|toggle <amount>
```

---

## Parameters / Arguments

* `inner`; Specifies that the gap value applies to **inner gaps** (i.e., spacing between adjacent windows/containers).
* `outer`; Specifies that the gap value applies to **outer gaps** (i.e., spacing between windows and the workspace edges / screen boundary).
* `horizontal`; Alternative to specifying both left and right outer gaps. Sets horizontal outer gaps.
* `vertical`; Alternative to specifying both top and bottom outer gaps. Sets vertical outer gaps.
* `top`, `right`, `bottom`, `left`; Specify outer gaps on specific sides of the workspace. Useful for finely controlling edge spacing.
* `<amount>`; The amount of spacing in pixels. This is the size of the gap.
* (When using runtime modifications) `all` or `current`; determines the scope: `all` affects all workspaces; `current` affects only the current workspace.
* (When using runtime modifications) `set`, `plus`, `minus`, `toggle`; operations to assign or modify gaps relative to their current value. `set` assigns a new gap size, `plus` adds, `minus` subtracts, `toggle` toggles (on/off) a gap value.

---

## Behavior and Effects

* When used with the simpler syntax (`gaps inner <amount>`, `gaps outer <amount>`, etc.) **in the configuration file**, the directive sets the *default* gap values for **new workspaces** that do not have custom gap settings.
* Outer gaps add on top of inner gaps. Thus the total space between a window and the screen edge will be (inner gap + outer gap) if both are non-zero.
* Negative values for outer gaps are allowed. This allows one to reduce or even invert the outer gap, which may be useful if you want windows closer to screen edges (or to compensate for UI elements like panels).
* When using runtime commands (`swaymsg …`), you can dynamically adjust gaps for `all` or `current` workspaces. This results in immediate effect for those workspaces.
* **Limitation / quirk**: The default (non-runtime) gap settings do **not** apply retroactively to already existing workspaces. That is, if you place `gaps inner 10` in your config and reload, existing workspaces may not show the changes.
* For per-workspace gap settings, one can use the `workspace <ws> gaps ...` directive. Those settings apply when the workspace is created.

---

## Usage Examples

**Terminal Example (runtime change):**

```bash
swaymsg gaps inner all set 15
swaymsg gaps outer current minus 5
```

This sets inner gaps to 15px on all workspaces, and decreases outer gaps by 5px on the current workspace.

**Config File Example:**

```conf
# Default gaps for all new workspaces
gaps inner 10
gaps outer 5

# Specific outer gaps per side
gaps left 20
gaps right 20

# For workspace 3: no gaps
workspace 3 gaps inner 0
workspace 3 gaps outer 0
```

---

## Related Commands

* `workspace <name> gaps inner|outer|horizontal|vertical|top|right|bottom|left <amount>`; sets gaps for a workspace when it is created.
* `smart_gaps on|off|inverse_outer`; a companion setting: when turned on, it disables gaps automatically when there is only one window/container in the workspace.
* `hide_edge_borders [--i3] none|vertical|horizontal|both|smart|smart_no_gaps`; often used in conjunction with gaps, to hide borders at screen edges when windows are aligned to edges.

---
