# `output`

## Overview

The `output` command in Sway is used to **manage and configure display outputs** (monitors) dynamically. It allows users to control settings such as resolution, refresh rate, scaling, position, and enable/disable state of each output. This command is primarily used for **workspace control** and **system execution**, particularly in multi-monitor setups or when adjusting display properties on the fly.

---

## Syntax

```
output <name> [options...]
```

Where `<name>` is the identifier of the output (e.g., `DP-1`, `HDMI-A-1`, `eDP-1`). `options` are directives that modify the output’s configuration.

---

## Parameters / Arguments

+ `<name>`: The name of the output to configure, as recognized by Sway (use `swaymsg -t get_outputs` to list available outputs).  
+ `enable | disable`: Enables or disables the specified output.  
+ `resolution <width>x<height>`: Sets the resolution of the output. Example: `resolution 1920x1080`.  
+ `position <x> <y>`: Places the output at the specified coordinates relative to the layout origin.  
+ `scale <factor>`: Sets a scaling factor for HiDPI adjustments. Example: `scale 1.5`.  
+ `transform <normal|90|180|270|flipped|flipped-90|flipped-180|flipped-270>`: Rotates or flips the display output.  
+ `refresh <rate>`: Sets the display refresh rate in Hz. Example: `refresh 60`.  
+ `background <path> <mode>`: Sets a background image and display mode (e.g., `stretch`, `fit`, `fill`, `center`).  

---

## Behavior and Effects

- Configures the display output immediately without requiring a Sway restart.  
- When enabling or disabling outputs, Sway automatically moves workspaces associated with that output to active displays.  
- Changing resolution, scaling, or refresh rate dynamically adjusts the output, and all windows are repositioned according to the new layout.  
- Invalid parameters or unsupported modes produce an error in Sway’s log; the previous configuration remains unchanged.  
- Using `output <name>` without any options prints the current configuration for that output.  

---

## Usage Examples

### Terminal Example 1: Enable and configure an output

```bash
swaymsg output HDMI-A-1 enable resolution 1920x1080 position 1920 0 scale 1.0
```

This enables the HDMI-A-1 output, sets its resolution to 1920x1080, positions it to the right of the primary monitor, and applies standard scaling.

### Terminal Example 2: Disable an output

```bash
swaymsg output DP-1 disable
```

Disables the DP-1 monitor, moving any active workspaces to remaining displays.

### Config File Example

```bash
output eDP-1 resolution 1920x1080 scale 1.0
output HDMI-A-1 resolution 2560x1440 position 1920 0 scale 1.5
```

This sets the laptop display to standard resolution and the external monitor to higher resolution with scaling, positioning it to the right of the laptop screen.

---

## Related Commands

* `swaymsg -t get_outputs`: Query current outputs and their status.
* `workspace`: Often used together with `output` to assign workspaces to specific displays.
* `reload`: After changing default output configurations in the config file, `reload` applies changes without restarting Sway.
* `move container to output <name>`: Moves a focused window to a specified output.

---
