# `swaymsg`

## Overview

`swaymsg` is a command-line tool that communicates with a **running Sway compositor** over its IPC socket. Its primary purpose is to *issue Sway commands or query state* at runtime. Because of this, it's used for **system execution**, **window/workspace management**, and **automation**. You can execute the same commands you put in your Sway config, but dynamically, or fetch JSON‑encoded state (workspaces, layout tree, inputs, outputs, etc.) via IPC.

---

## Syntax

```
swaymsg [options...] [message]
```

Where:

- `options` are flags that control how `swaymsg` behaves (e.g., output format, socket path, message type).  
- `message` is either a Sway command (like `move container to workspace 2`) or a *query type* such as `get_tree`.

---

## Parameters / Arguments

Here is a breakdown of the key options and arguments:

+ `-h, --help`: Show help message and exit.
+ `-m, --monitor`: When used with the `subscribe` type, keeps monitoring events, printing as they come.
+ `-p, --pretty`: Produce pretty / human-readable output (if not using a TTY).
+ `-q, --quiet`: Send the IPC message, but do not print Sway’s reply.
+ `-r, --raw`: Return raw JSON output, even when running in a TTY.
+ `-s, --socket <path>`: Use a specific IPC socket path instead of auto-detecting.
+ `-t, --type <type>`: Specify the type of IPC message (e.g., command, get_tree, get_outputs).
+ `-v, --version`: Print the version of `swaymsg` and exit.

**IPC Message Types** (`-t` / `--type`):  
These define what kind of payload you’re sending or querying. Common types include:  

- `command`: Executes a regular Sway command, the same as writing it in the config or via a keybinding.
- `get_workspaces`: Fetches a list of workspaces along with their metadata.
- `get_inputs`: Retrieves information about input devices such as keyboards and mice.
- `get_outputs`: Retrieves information about connected outputs (monitors).
- `get_tree`: Returns a JSON-encoded layout tree of all containers, windows, outputs, and workspaces.
- `get_seats`: Provides information about seats.
- `get_marks`: Lists all currently defined marks in Sway.
- `get_bar_config`: Fetches the configuration of the Sway status bar.
- `get_version`: Returns version information for the running Sway instance.
- `get_binding_modes`: Lists all defined binding modes.
- `get_binding_state`: Returns the current binding state, indicating which mode is active.
- `get_config`: Retrieves the currently loaded Sway configuration.
- `send_tick`: Sends a “tick” event to Sway IPC clients.
- `subscribe`: Subscribes to events, such as window, workspace, or output changes, to receive real-time IPC notifications.

---

## Behavior and Effects

- When `message` is a **Sway command**, `swaymsg` sends it over IPC and Sway executes it immediately, as though it came from your config or a keybinding.  
- When using **query types** (e.g., `-t get_tree`), `swaymsg` returns structured data (usually JSON) describing the current state.  
- By default, `swaymsg` pretty-prints replies when the output is a TTY; you can force JSON with `--raw`.
- `swaymsg` locates the IPC socket via the environment variable `SWAYSOCK` (or, if unset, falls back to a default) unless overridden by `--socket`.
- For messages that start with a hyphen (e.g., a command like `-someflag`), you must prefix with `--` so `swaymsg` doesn’t interpret it as its own option.
- Exit codes:  
  + `0`; success
  + `1`; syntax error, IPC socket problem, or cannot parse reply
  + `2`; Sway returned an error when processing the command (e.g., invalid command)

---

## Usage Examples

### Example 1: Query workspace list (terminal)

```bash
swaymsg -t get_workspaces
```

This will print a JSON array describing each workspace (its name, whether it is focused, its layout, etc.). Useful for automation or scripting.

### Example 2: Query the full layout tree (terminal)

```bash
swaymsg -t get_tree | jq '.'
```

This gives you a detailed hierarchical representation of outputs, workspaces, containers, and windows, very useful for introspection (for example, when writing scripts that inspect or act based on layout).

### Example 3: Execute a Sway command (terminal)

```bash
swaymsg move container to workspace number 3
```

This moves the currently focused container (window) to workspace “3”.

### Example 4: Use in a Sway config (via bindsym)

In your `~/.config/sway/config`:

```text
bindsym $mod+Shift+3 exec move container to workspace number 3
```

This binds `Mod + Shift + 3` to move the focused window to workspace 3.

---

## Related Commands

Here are some commands and concepts commonly used together or in relation to `swaymsg`:

* **Sway config commands**: Many of the commands you send via `swaymsg` are the same as those in your `~/.config/sway/config` (e.g., `move`, `focus`, `workspace`).
* **IPC / Query types**:

  * `swaymsg -t get_outputs`; to query outputs (monitors)
  * `swaymsg -t subscribe`; to listen to events (workspaces, windows, etc.)
* **Scripting tools**: `jq` is often paired with `swaymsg -t get_tree` or `get_workspaces` to filter and act on JSON.
* **For configuration automation**: Sway marks (`mark`), layout commands (`split`, `resize`), or workspace manipulation (`workspace`, `move`) are frequently sent via `swaymsg`.

---
