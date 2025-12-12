# `bar`

## Overview

The `bar` command in Sway is used to configure and control the status bar (swaybar or a custom bar) via the Sway configuration file. It is primarily for **system execution** (launching the bar process) and **layout manipulation** (managing visibility, positioning, and appearance of the bar).

---

## Syntax

```
bar [<bar-id>] <bar-subcommands…>
```

Where `<bar-subcommands>` may include directives such as `swaybar_command`, `mode`, `hidden_state`, `position`, `output`, and so on.

---

## Parameters / Arguments

Here are the key parameters allowed inside a `bar { … }` block in the Sway config:

+ `id <bar_id>`: Assigns an identifier to the bar instance.  
+ `swaybar_command <command>`: Specifies the command to run for the bar. By default, this is `swaybar`, but you can override it (e.g., to run `waybar`).
+ `mode dock|hide|invisible|overlay [<bar-id>]`: Controls the visibility behavior of the bar:
  - `dock`: permanently visible.
  - `hide`: hides the bar unless a modifier key is pressed, depending on `hidden_state`.
  - `invisible`: the bar is permanently hidden.
  - `overlay`: permanently visible on top of windows; transparent to input events.
+ `hidden_state hide|show [<bar-id>]`: Determines how `hide` mode behaves:
  - `hide`: the bar is hidden by default, shown only when the modifier key is pressed or on urgency.
  - `show`: the bar is permanently visible on top of the active workspace.
+ `modifier <Modifier>|none`: Defines which key must be pressed to unhide the bar in `hide` mode. Default is `Mod4`.
+ `position top|bottom`: Determines where the bar appears on the screen. Default: bottom.
+ `output <output>|*`: Restricts the bar to certain output(s). Use `*` to reset to all outputs.
+ `font <font>`: Sets the font for the bar, using a Pango font description.
+ `gaps <all> | <horizontal> <vertical> | <top> <right> <bottom> <left>`: Defines the margin or “gap” between the bar and the screen edge.
+ `height <height>`: Sets the height (in pixels) of the bar. A value of `0` makes the height match the font size.
+ `separator_symbol <symbol>`: Defines a custom symbol to separate blocks in the bar.
+ `status_command <status command>`: Executes a shell command (via `sh -c`) whose stdout is rendered on the bar. You may use the JSON protocol for richer status lines.
+ `status_edge_padding <padding>`: Horizontal padding on the right edge of the status area. Default: `3` (scaled).
+ `status_padding <padding>`: Vertical padding for status lines. Default: `1` (scaled). If `0`, blocks can occupy full bar height.
+ `binding_mode_indicator yes|no`: Whether to show a small indicator when Sway is in a non-default binding mode. Default: yes.
+ `wrap_scroll yes|no`: If scrolling through workspace buttons via the scroll wheel should wrap around. Default: no.
+ `workspace_buttons yes|no`: Whether workspace buttons are shown on the bar. Default: yes.
+ `workspace_min_width <px> [px]`: Minimum width for workspace buttons. Default: 0.
+ `strip_workspace_numbers yes|no`: If `yes`, workspace numbers are removed, leaving only their names. Default: no.
+ `strip_workspace_name yes|no`: If `yes`, workspace names are omitted, leaving only numbers (or custom names).
+ `unbindsym [--release] button[1-9] | <event-name>`: Unbinds a mouse button action from the bar.
+ `unbindcode [--release] <event-code>`: Unbinds based on raw event code from libinput.
+ **Tray controls** (for system tray):
  + `tray_bindcode <event-code> <action>`  
  + `tray_bindsym button[...] <action>`  
  + `tray_padding <px> [px]`  
  + `tray_output none|<output>|*`  
  + `icon_theme <name>`
+ **Color definitions** (within a `colors { … }` sub-block):
  + `active_workspace`, `inactive_workspace`, `urgent_workspace`, `binding_mode`; each takes three hex values: border, background, and text color.

---

## Behavior and Effects

- When Sway starts up, it reads one or more `bar { … }` blocks in its configuration to initialize bar instances.
- The `swaybar_command` determines which process is launched for the bar. If you override it (e.g., with `waybar`), Sway will start that instead of its default `swaybar`.
- The `mode` and `hidden_state` control how the bar is shown:
  - `dock` forces the bar to always occupy its edge.
  - `hide` means it's not always visible; whether it stays hidden or shows when the modifier is pressed depends on `hidden_state`.
  - `invisible` makes the bar never render (useful if you want no bar).
  - `overlay` places the bar above all windows, but does not allow pointer events to pass through.
- Mouse bindings (e.g., `bindsym button1 …`) apply inside the bar: you can control what happens when users click various parts of the bar.
- The status line is dynamically updated via `status_command`: whenever the command outputs a line, the bar updates.
- Appearance (font, padding, separators) can be fully customized. Defaults are provided if you omit them.
- For systems with multiple outputs, you can restrict a bar to a subset of displays via the `output` directive.

---

## Usage Examples

**Terminal / IPC Example:**

You can control the bar at runtime using `swaymsg`. For instance, to toggle the bar between `hide` and `dock`:

```bash
swaymsg bar mode toggle
```

Or to set a bar with ID `bar-0` to `invisible`:

```bash
swaymsg bar mode invisible bar-0
```

---

**Sway Config File Example:**

```text
bar {
    id bar-0
    swaybar_command waybar
    mode hide
    hidden_state show
    modifier Mod4
    position top
    output HDMI-A-1
    font pango:Monospace 10
    height 24
    separator_symbol |
    status_command while date +'%Y-%m-%d %H:%M:%S'; do sleep 1; done
    status_edge_padding 5
    status_padding 2
    binding_mode_indicator yes
    workspace_buttons yes
    wrap_scroll yes
    strip_workspace_numbers no

    colors {
      active_workspace   #ffffff #285577 #ffffff
      inactive_workspace #888888 #333333 #888888
      urgent_workspace   #ff0000 #880000 #ff0000
      binding_mode       #000000 #ffaa00 #000000
    }

    tray_output *
    tray_padding 2 2
    icon_theme "hicolor"
}
```

This configuration:

* Launches **Waybar** instead of sway’s bar.
* Puts the bar at the **top**, on a specific output.
* Uses a simple shell loop for the status.
* Enables workspace buttons, scroll wrapping, a binding-mode indicator, and a tray.

---

## Related Commands

* `exec` / `exec_always` in Sway config; often used to start external bars (e.g., Waybar) independently of the `bar` block.
* `workspace` / `output` commands; commonly combined with `bar … output …` to decide which monitor shows which bar.

---
