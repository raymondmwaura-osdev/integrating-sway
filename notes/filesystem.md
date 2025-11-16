# Filesystem Layout

## Overview

Sway is a tiling Wayland compositor inspired by i3, designed to run on Linux (and other Unix‑like OSes).
As such, its file‑system “footprint” includes:

1. Configuration (user and system)
2. IPC / runtime sockets
3. Auxiliary tool configurations (lockers, background, etc.)
4. Potential state or logs (though Sway itself is relatively stateless).

---

## Configuration Files

### Search Order and Priority

When starting, Sway will look for its configuration file in a defined precedence:

1. `~/.sway/config`
2. `$XDG_CONFIG_HOME/sway/config`; *recommended* location (if `XDG_CONFIG_HOME` is set)
3. `~/.i3/config` (for compatibility)
4. `$XDG_CONFIG_HOME/i3/config`
5. `/etc/sway/config` (system-wide default)
6. `/etc/i3/config` (legacy compatibility)

If `XDG_CONFIG_HOME` is not defined, it defaults to `~/.config/`.
If no config file is found in any of those locations, Sway raises an error.

**Best practice**, copy the default system config (usually in `/etc/sway/config`) into your user config directory (e.g., `~/.config/sway/config`), then edit from there.

---

### Config Directory Structure (User)

While Sway itself requires just one main config file, many users adopt a modular setup under `~/.config/sway/` to organize different aspects. For example, you may find sub‑directories under `~/.config/sway/` such as:

* `sway-input/`; input/keymap settings
* `sway-output/`; monitor/output configuration, background (wallpaper) declarations
* `sway-modes/`; different modes (like resize mode, move mode)
* `sway-idle/`; configuration for idle behaviors (e.g. via `swayidle`)
* `sway-theme/`; theming (colors, gaps etc.)

This modular layout is **not mandated** by Sway upstream, but is a common pattern.

Additionally, some Sway distributions include layered configuration via a `config.d/` directory: for instance, many distro packages include `/usr/share/sway/config.d/` or `/etc/sway/config.d/` for modular defaults.
User configs can also include these via an `include` directive in their config.

---

## Runtime Artifacts

### IPC Socket (SWAYSOCK)

Sway provides a Unix domain socket for its IPC (inter-process communication), similar to i3.
Key details:

* The **environment variable** `SWAYSOCK` points to the path of the IPC socket.
* For backwards compatibility, `I3SOCK` may also point to it.
* You can programmatically obtain the socket path by running:

  ```bash
  sway --get-socketpath
  ```

* The socket is typically created under the user’s runtime directory, for example under `$XDG_RUNTIME_DIR`.
* Tools/plugins (e.g., `swaymsg`, `swayipc`) rely on this socket to send commands or subscribe to events.

Be careful: the naming convention of the socket may vary slightly across boots (depending on process ID or user ID), so code that uses it may need to dynamically resolve the correct socket via `--get-socketpath` or by listing matching sockets in `$XDG_RUNTIME_DIR`.

---

## Auxiliary Tools and Their Configuration

Sway itself is a compositor, but it's commonly used with companion tools: **swayidle**, **swaylock**, **swaybg**, etc. Their configurations are typically separate from the main Sway config.

* **swaylock**:
  It looks for the config file in:

  * `~/.swaylock/config`
  * `$XDG_CONFIG_HOME/swaylock/config`
    Notably, swaylock does *not* generate a config by default, so you may need to create one.

* **swayidle**:
  This is usually invoked from within the Sway config via `exec` lines. Its configuration is managed separately (you typically point to a config file path when launching it).

* **swayimg**:
  (A lightweight image viewer designed to work under Sway.) Its configuration follows XDG patterns: it checks

  * `$XDG_CONFIG_HOME/swayimg/config`
  * `$HOME/.config/swayimg/config`
  * then system-level dirs `/etc/xdg/swayimg/config`

---

## State, Logs, and Persistence

* **State file**: Unlike some desktop environments, Sway does **not** maintain a persistent “state file” of which windows were open, or their session history. Sway just executes the commands defined in the config; it does not serialize a “session restore” state.
* **Logging**: While Sway logs to standard output / stderr when launched, any persistent logging (if enabled) depends on how it’s invoked (e.g., systemd service, journal). There is no canonical “Sway log directory” created by Sway itself.
* **Runtime directory**: Beyond the IPC socket, Sway generally uses the user’s `XDG_RUNTIME_DIR` for any per-session runtime artifacts.

---

## Common Pitfalls and Best Practices

1. **Duplicated configs / conflicting includes**: Because many distributions ship `config.d/` fragments, user configs should explicitly `include` only what they need to avoid overlap or conflicting directives.
2. **Incorrect `SWAYSOCK`**: Scripts using `swaymsg` or other IPC tools may fail if `SWAYSOCK` is not exported or if it's hardcoded incorrectly. It’s safer to dynamically query the socket path (`sway --get-socketpath`) before using it.
3. **XDG runtime dir**: Make sure `XDG_RUNTIME_DIR` is correctly set in your session. If not, the IPC socket may not be placed where you expect.
4. **Modular config layout**: While modular per-domain config (input, output, idle) is helpful, you need to maintain `include` directives in your main config; otherwise, Sway may ignore these files.

---
