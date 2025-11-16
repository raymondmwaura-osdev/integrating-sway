# `reload`

## Overview

The `reload` command in Sway instructs the running compositor to **reload its configuration file** without restarting the session. This allows users to apply changes to keybindings, workspace rules, layout settings, or other configuration parameters dynamically. It is primarily used for **system execution** and **configuration management**, enabling rapid iteration on the Sway setup without disrupting active windows or workspaces.

---

## Syntax

```
reload
```

This command has no parameters or arguments; it operates directly on the currently loaded configuration file.

---

## Behavior and Effects

- Upon execution, Sway parses the configuration file and applies any updated settings.  
- Keybindings, workspace assignments, bar configurations, and layout directives are refreshed immediately.  
- Active windows remain open, and their positions and layouts are preserved unless explicitly modified by the new configuration.  
- If the configuration contains syntax errors, Sway logs an error message, and the previous configuration remains active; the session is **not terminated**.  
- This command is useful for testing configuration changes iteratively, avoiding the need for a full restart of the Sway session.

---

## Usage Examples

### Terminal Example

```bash
swaymsg reload
```

This sends the `reload` command via IPC to the currently running Sway instance, immediately applying any changes made to the configuration file.

### Config File Example

Although `reload` is most often invoked manually, it can also be bound to a key combination within the Sway configuration file:

```bash
bindsym $mod+Shift+c reload
```

This binds `Mod + Shift + C` to reload the Sway configuration at any time.

---

## Related Commands

* `restart`: Fully restarts the Sway session while preserving the layout of open windows.
* `exit`: Exits the Sway session entirely.
* `swaymsg reload`: The IPC command equivalent for sending `reload` programmatically.
* `swaymsg restart`: Allows scripted restarts of Sway with updated configurations.

---
