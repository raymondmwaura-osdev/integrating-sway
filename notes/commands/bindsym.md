# `bindsym`

## Overview

`bindsym` is a Sway configuration directive used to bind keyboard (or mouse) key combinations to specific Sway commands. It is primarily used for **system execution** (such as launching applications), **window management** (e.g., focusing or moving containers), **workspace control** (e.g., switching workspaces), and **layout manipulation** (e.g., toggling layouts). It forms the backbone of how users interact with Sway through keyboard shortcuts.

---

## Syntax

```
bindsym [--whole-window] [--border] [--exclude-titlebar] [--release] [--locked]
[--to-code] [--input-device=<device>] [--no-warn] [--no-repeat]
[--inhibited] [Group<1-4>+]<key combo> <command>
```

---

## Parameters / Arguments

+ `--whole-window`: For mouse bindings, allows the binding to trigger when the cursor is anywhere over the window; including its content, title bar, and border.
+ `--border`: For mouse bindings, include the window border area in the region that triggers the binding.
+ `--exclude-titlebar`: For mouse bindings, exclude the title bar from the clickable region.
+ `--release`: The command is executed when the key combination is *released*, rather than when pressed.
+ `--locked`: The binding remains active even when a screen-locking program is running.
+ `--to-code`: Treat the key combo as key codes rather than key symbols. This is useful when working with different keyboard layouts.
+ `--input-device=<device>`: Restrict the binding to a specific input device (e.g., a particular keyboard).
+ `--no-warn`: Suppress warnings if this binding overrides an existing one.
+ `--no-repeat`: Prevents the command from repeating if the key is held down. By default, repeated key presses (auto-repeat) trigger the command repeatedly.
+ `--inhibited`: Allows the binding to execute even when a keyboard shortcut inhibitor is active (e.g., in a remote‑desktop or virtualization scenario).
+ `Group<1-4>+`: (Optional) Specifies a group number (1-4) in which the binding is active.
+ `<key combo>`: The key or key combination, using XKB key names (e.g., `Mod4+Return`, `XF86AudioRaiseVolume`).
+ `<command>`: The Sway command to run when the key combo is triggered (for example, `exec`, `focus`, `move`, `layout`, etc.).

---

## Behavior and Effects

When `bindsym` is processed (typically via the Sway configuration file), it registers a key binding. When the specified key combo is pressed (or released, if `--release` is used), Sway executes the bound command.

- By default, a binding will not run if the screen is locked; unless `--locked` is set.
- If `--no-repeat` is not specified, holding down the key combo will repeatedly trigger the command according to the input device’s repeat rate.
- For mouse bindings, the region that triggers the binding depends on `--whole-window`, `--border`, and `--exclude-titlebar`. Without those options, mouse bindings are only active when clicking the title bar.
- The `--to-code` flag makes the binding layout-independent by translating key symbols into their physical key codes, which helps when using multiple keyboard layouts.
- The `--input-device` parameter is useful for per-device bindings (e.g., having different shortcuts on separate keyboards).
- Grouping allows for separate layers of bindings; a binding defined in a specific group only applies when that group is active.
- Suppressing warnings (`--no-warn`) is helpful when intentionally overriding default or existing bindings.

---

## Usage Examples

1. **Launching a terminal**

   ```text
   bindsym $mod+Return exec alacritty
   ```

2. **Switching workspaces**

   ```text
   bindsym $mod+1 workspace 1
   bindsym $mod+2 workspace 2
   ```

3. **Resizing mode using `--release`**

   ```text
   bindsym --release $mod+r mode "resize"
   ```

4. **Layout manipulation**

   ```text
   bindsym $mod+s layout stacking
   bindsym $mod+w layout tabbed
   bindsym $mod+e layout toggle split
   ```

---

## Related Commands

* `bindcode`: Similar to `bindsym`, but binds by keycode instead of key symbols.
* `bindswitch`: Bind commands to hardware switch events (e.g., lid close/open).
* `mode`: Define custom input modes (often used in conjunction with `bindsym`) to create modal keybindings.
* `exec`: The Sway command typically used after `bindsym` to run programs.
* `focus`, `move`, `layout`, `workspace`, `kill`: Common Sway commands that are often targets of `bindsym`.

---
