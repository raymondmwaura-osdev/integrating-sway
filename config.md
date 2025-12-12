# Writing Sway Config

## Conceptual Foundations

1. **Sway as a Declarative System**
   Sway’s configuration is fundamentally **declarative**: the config file consists of a series of commands that Sway executes at startup. These commands define behavior (e.g., keybindings, window layout, input settings, outputs) rather than writing imperative control logic.

2. **i3 Compatibility**
   Sway inherits much of its configuration syntax from the i3 window manager, meaning that many i3‑style configuration paradigms work directly in Sway.

3. **Modularity via `include`**
   To manage complexity, Sway supports modular configuration: one config file can include other config fragments. This is done via the `include` directive.

4. **Block‑structured Commands**
   Several commands can be grouped into blocks, allowing hierarchical configuration (e.g., output-specific settings).

5. **Line Continuation**
   For readability, very long commands can be split across lines using a backslash (`\`) at the end of a line.

---

## Configuration File Location

* By default, Sway reads configuration from one of the following (in order):

  1. `~/.sway/config`
  2. `$XDG_CONFIG_HOME/sway/config` (if `XDG_CONFIG_HOME` is set, otherwise defaults to `~/.config/sway/config`)
  3. Fallbacks for backwards compatibility: `~/.i3/config`, `$XDG_CONFIG_HOME/i3/config`
  4. System-wide defaults: `/etc/sway/config` (or legacy `/etc/i3/config`).
* You can override the path to the config file when launching Sway using the `-c, --config` option.
* Sway also supports a “validate” mode (`--validate`) which checks the syntax of a config file and exits without starting the compositor.

---

## Core Syntax and Conventions

Here are fundamental syntactic rules and idioms you will encounter when writing a Sway config:

1. **Tokens and Arguments**

   * Commands are split by whitespace.
   * If an argument contains spaces, you must quote it using `"..."` or `'...'`.
   * You can chain multiple commands on a single line using commas (`,`) or semicolons (`;`), though the semantics differ:
     * `,` retains criteria (if any) from prior command.
     * `;` resets criteria so the next command does not inherit.

2. **Blocks**

   * Block syntax: `command { <subcommands …> }` allows grouping.
   * Within a block, the “parent” command is implicitly prepended to each sub‑command. For example:
     ```text
     output eDP-1 {
       resolution 1920x1080
       background ~/wallpaper.png fill
     }
     ```
     is equivalent to:
     ```text
     output eDP-1 resolution 1920x1080
     output eDP-1 background ~/wallpaper.png fill
     ```

3. **Include Directive**

   * `include <path>` allows you to pull in another config file. The path can be absolute or relative, and supports shell‑style expansion.
   * A given file is only included once; if the same include is repeated, further attempts are ignored.

4. **Line Continuation**

   * To split a long command into multiple lines, append a backslash (`\`) at the end of the line:
     ```text
     bindsym Shift+XF86AudioRaiseVolume exec \
         pactl set-sink-volume @DEFAULT_SINK@ +5%
     ```

---

## Key Components of a Typical Sway Config

A practical Sway configuration usually comprises several conceptual areas, each defined by commands or command blocks. Below are some of the most common:

1. **Keybindings (`bindsym`)**

   * Used to bind keyboard shortcuts to actions (e.g., launching an application, changing window layout).
   * Example:
     ```text
     bindsym $mod+Return exec alacritty
     bindsym $mod+Shift+q kill
     ```
2. **Mode Switching**

   * Sway supports “modes” (e.g., a special keybinding mode for resizing or moving windows).
   * Modes can be defined via `mode <name> { … }` and then bound to a key.
3. **Window Rules (`for_window`)**

   * You can apply rules that match windows (by title, class, instance, etc.) and execute commands when they appear.
   * Example:
     ```text
     for_window [class="Firefox"] floating enable
     ```
   * (Note: some commands are only allowed in certain contexts; for instance, floating state is usually set via `bindsym` or `for_window`, not directly globally.)
4. **Output Configuration**

   * Using `output <name> { ... }`, you can configure monitor-specific settings: resolution, position, background, scaling, etc.
5. **Input Devices**

   * Through `input` blocks, you control keyboard layout, pointer acceleration, device-specific options.
   * Example (from Debian guide):
     ```text
     input * {
       xkb_layout "us,de,ru"
       xkb_variant "colemak,,typewriter"
       xkb_options "grp:win_space_toggle"
     }
     ```

6. **Bars and Status**

   * `bar { ... }` defines a status bar. Inside you set subcommands for that bar (e.g. modules, position, colors).
7. **General Settings**

   * Commands like `default_orientation`, `focus_follows_mouse`, or `workspace_auto_back_and_forth` set global behavior.
8. **Autostart / Exec**

   * You can run programs at startup using `exec` (or `exec_always`) inside the config.
   * Example:
     ```text
     exec mako
     exec swaybg -i ~/Pictures/wallpaper.jpg -m fill
     ```

---

## Validation and Error Handling

* Before launching a full Sway session, you can test your configuration syntax with:

  ```bash
  sway --validate [-c /path/to/your/config]
  ```

  This causes Sway to parse your config and report any syntax errors without starting the compositor.
* In case of incorrect commands or malformed blocks, Sway may refuse to start, or behavior may differ from your expectations.
* It is common (and recommended) to maintain a working base config (e.g., copied from `/etc/sway/config`) and layer customizations via `include`.

---

## Best Practices for Config Design

1. **Use a Modular Structure**

   * Split your config into logical fragments (e.g., `input`, `output`, `bindings`, `modes`) and include them from a central file. This enhances maintainability.
2. **Leverage Blocks**

   * Use block syntax (`command { ... }`) where applicable to avoid repeating the same prefix.
3. **Comment Liberally**

   * Since your config is a declarative script, annotating your decisions (why a binding, why a particular mode) helps readability.
4. **Use `--validate` Often**

   * After edits, run validation to catch syntax errors immediately, rather than debugging a broken session.
5. **Stay Close to Defaults (Initially)**

   * When starting out, begin by copying the system-supplied default config (e.g., `/etc/sway/config`) and modifying incrementally.
6. **Version Control Your Config**

   * Since your config defines nearly all Sway behavior, keep it under version control (e.g., Git) to track changes, roll-back, and experiment safely.

---

## Limitations and Caveats

* **No Runtime Conditional Inclusion**: The `include` directive in Sway’s config is static; you cannot dynamically include different fragments based on runtime environment or variables. You can work around this by using symlinks + `reload`, but environment-variable-based conditional includes are not supported.
* **Error Messages Are Sparse**: If there is a syntax error, the feedback may be limited, forcing you to rely on `sway --validate` or careful incremental edits.
* **Command Scope**: Not all Sway commands are valid or meaningful in all contexts. Some commands make sense in `for_window`, some only at global scope, some only under `bar`, etc. Refer to the Sway manpages (especially `sway(5)`) for detailed command scopes.
* **Editor Support**: Syntax highlighting or language support in editors may lag behind or have minor mismatches; you may encounter false warnings in e.g. Vim.

---
