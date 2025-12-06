# `move`

## Overview

The `move` command in Sway is used for **window/container management** and **workspace or output relocation**. Its primary purpose is to reposition the currently focused container (or workspace, in some variants); either by changing its coordinates (for floating containers), or by sending it to another workspace, output, or mark. Thus it operates in the context of managing layout, workspace assignment, and container positioning within Sway.

---

## Syntax

Possible forms of the `move` command in Sway include:

```
move [absolute] position <pos_x> [px|ppt] <pos_y> [px|ppt]
move [absolute] position center
move position cursor|mouse|pointer

move [container|window] [to] mark <mark>

move [--no-auto-back-and-forth] [container|window] [to] workspace [number] <name>
move [container|window] [to] workspace prev|next|current
move [container|window] [to] workspace prev_on_output|next_on_output
move [container|window] [to] workspace back_and_forth

move [container|window] [to] output <name-or-id>|current
move [container|window] [to] output up|right|down|left

move [container|window] [to] scratchpad

move workspace [to] output <name-or-id>|current
move workspace [to] output up|right|down|left
```

---

## Parameters / Arguments

+ `absolute`: When used with the `position` subcommand, indicates that the coordinates are relative to all outputs (i.e., global coordinate space), not just the current workspace.  
+ `position <pos_x> [px|ppt] <pos_y> [px|ppt]`: Specifies the target x- and y-coordinates for the focused container. Units may be in pixels (`px`) or percentage points (`ppt`). If units are omitted, defaults to pixels. This is only meaningful for floating containers.
+ `position center`: Center the focused container. With `absolute`, centers relative to all outputs; otherwise centers in the current workspace.
+ `position cursor|mouse|pointer`: Move the container so that it is centered on the current cursor/mouse pointer position. Only affects floating containers.
+ `container|window`: (Optional) specifies that the operation should apply to the focused container/window. In Sway, these keywords are optional because the command acts on the focused container by default.
+ `to`: (Optional) syntactic keyword used before specifying a destination (workspace, output, mark, etc.).  
+ `mark <mark>`: Moves the focused container to the specified mark. Marks refer to previously assigned marks in Sway used to jump or send windows to labeled containers.
+ `workspace [number] <name>`: Moves container to a workspace named `<name>`. The optional `number` keyword allows matching workspaces by number even if their “name” is different.
+ `workspace prev|next|current`: Moves container to the previous, next, or current workspace on the *same output*. If no further workspaces remain on that output, the container may move to the previous/next output.
+ `workspace prev_on_output|next_on_output`: Moves container to previous or next workspace on the same output, wrapping around at bounds.
+ `workspace back_and_forth`: Moves container to the previously focused workspace (useful to toggle between two workspaces).
+ `output <name-or-id>|current`: Moves the focused container (or workspace) to the specified output (monitor). Using `current` returns it to the current output.
+ `output up|right|down|left`: Moves container or workspace to the next output in the given direction. Useful for multi-monitor setups.
+ `scratchpad`: Moves container to the “scratchpad”; a hidden storage area for windows that can be recalled later.
+ `--no-auto-back-and-forth`: When used with workspace moves, disables automatic “back-and-forth” behavior even if `workspace_auto_back_and_forth` is enabled. Useful to avoid unintended backtracking.

---

## Behavior and Effects

When executed, `move` acts on the **currently focused container/window** (unless criteria or marks are used). Depending on subcommand:

- For `position`: If the container is floating, its position within the workspace (or global output area, if `absolute`) changes to the specified coordinates. If no units are supplied, pixels are assumed. If `position center`, the container is centered in the workspace or across outputs. If `position cursor|mouse|pointer`, the container moves to center at the pointer location. These position-based moves do **not** affect tiled containers in a meaningful way (tiling ignores arbitrary x/y positioning).
- For workspace moves: The container is detached from the current workspace and placed into the target workspace (which may be identified by name, number, or relation such as next/prev). If the target workspace does not exist, Sway will create it (especially if number is used and corresponds to a new workspace).
- For output moves: The container (or workspace) is relocated to a different output (e.g., a different monitor). In a multi-monitor environment, this allows dynamic placement of windows or entire workspaces on different displays.
- For scratchpad: The container is hidden from the current workspace and stored in a hidden “scratchpad” for later retrieval.
- For mark-based moves: Sway will reparent the container to the container designated by the given mark. Useful for complex window layout and navigation workflows.

Special conditions / edge cases:

- Using `absolute` with percentage units (`ppt`) is invalid: `absolute` may not be combined with ppt units.
- The keywords `container` or `window` are optional; Sway assumes the focused container.
- If the workspace move target references a number that does not yet exist (and there is no existing workspace with that number), Sway will create a new workspace with that name.
- Using `--no-auto-back-and-forth` suppresses the automatic “return to previous workspace” behavior that may happen when `workspace_auto_back_and_forth` is enabled. Omission of this flag relies on the global `workspace_auto_back_and_forth` setting.

---

## Usage Examples

**Terminal Example 1; Move floating container to specific coordinates**  

```bash
swaymsg move position 100 px 200 px
```

This moves the focused (floating) container to x = 100 px, y = 200 px within the current workspace.

**Terminal Example 2; Send focused container to workspace “3”**

```bash
swaymsg move container to workspace 3
```

**Config File Example 1; Keybinding: move focused window to workspace 2**

```bash
bindsym $mod+Shift+2 move container to workspace 2
```

**Config File Example 2; Keybinding: move focused window to next output (e.g. on multi-monitor setup)**

```bash
bindsym $mod+Shift+o move container to output right
```

---

## Related Commands

* `focus`: Used to change focus between containers; often used alongside `move` in workflows.
* `workspace`: Used to switch workspaces; frequently combined with `move container to workspace` to both move and follow a window to the new workspace (since `move` alone does not change focus/workspace).
* `assign` (in configuration): A directive to automatically send new windows matching criteria to a workspace or output (equivalent to `for_window … move …`).

---
