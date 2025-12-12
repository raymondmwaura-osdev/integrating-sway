# Waybar

## What Is Waybar?

Waybar is a **highly customizable status bar** designed for **Wayland** compositors, particularly those based on *wlroots* (e.g., Sway, Hyprland).
It provides real-time system information (such as battery, clock, network, audio) using a modular architecture.
The bar’s design is lightweight and focuses on performance, minimizing overhead while offering rich customization capabilities.

---

## Key Features

1. **Modular Architecture**
   * Each component (or “widget”) is a module (e.g., clock, battery, network), which can be added, removed, or reordered as needed.
2. **JSONC Configuration**
   * Waybar uses a JSON variant (JSONC; JSON with comments) to define its configuration.
3. **CSS Styling**
   * The appearance is styled via CSS, allowing full control over colors, fonts, spacing, and other visual aspects.
4. **Low Overhead**
   * Because it’s written for Wayland and optimized for performance, it is lightweight and fast.
5. **Real-Time Updates**
   * Modules can update at configurable intervals to reflect live system states (e.g., CPU usage, volume).
6. **Custom Scripts**
   * You can integrate custom scripts (bash, Python, etc.) to produce arbitrary output on the bar.

---

## Installation

The installation procedure depends on your Linux distribution and the compositor you're using.

1. **Using Your Package Manager**

   * On **Ubuntu / Debian**:
     ```bash
     sudo apt install waybar
     ```
     (You might also use a PPA for newer versions.)
   * On **Arch Linux**:
     ```bash
     sudo pacman -S waybar
     ```
   * On **Fedora**:
     ```bash
     sudo dnf install waybar
     ```
2. **From Source**

   * Clone the Git repository, build with Meson, then install with Ninja.
3. **Compositor Integration**

   * For **Sway**, you can tell Sway to use Waybar by specifying in your Sway config:
     ```text
     bar {
       swaybar_command waybar
     }
     ```
   * For **Hyprland**, you can add an “exec-once” directive:
     ```
     exec-once = waybar
     ```

---

## Configuration

Waybar’s behavior and appearance are determined by two primary files, stored by default in: `~/.config/waybar/`
These are:

1. `config` (or `config.jsonc`): defines modules, layout, update intervals, and module-specific settings.
2. `style.css`: defines the visual style (using CSS); colors, fonts, spacings, etc.

### Example Basic Configuration

Here is a minimal example of a `config.jsonc` file:

```jsonc
{
  "layer": "top",
  "modules-left": ["sway/workspaces", "sway/mode"],
  "modules-center": ["sway/window"],
  "modules-right": ["battery", "clock"],

  "battery": {
    "format": "{capacity}% {icon}",
    "format-icons": ["", "", "", "", ""]
  },

  "clock": {
    "format-alt": "{:%a, %d. %b  %H:%M}"
  },

  "sway/window": {
    "max-length": 50
  }
}
```

Key fields:

* **layer**: defines whether the bar is on top or bottom.
* **modules-left/center/right**: specify which modules go in which part of the bar.
* **module-specific settings**: for “battery” and “clock” in the example, you provide more detail for formatting and icon usage.

---

## Styling with CSS

Because Waybar supports CSS, you can greatly customize its appearance. Typical things you might style:

* Module containers (e.g., `.modules-left`, `.modules-right`)
* Individual module elements (e.g., `#cpu`, `#clock`)
* Fonts, padding, margins, background colors, hover/tooltip effects
* You reload the CSS by restarting Waybar or using its reload functionality.

---

## Advanced Usage

1. **Custom Scripts / Modules**
   * You can define “custom/…” modules that execute a script at a given interval and display its output.
2. **Multiple Configuration Profiles**
   * You may maintain different configuration files (e.g., `config-work.jsonc`, `config-home.jsonc`) and launch Waybar with a specific one using the `-c` flag.
3. **Toggling / Hiding the Bar**
   * It is possible to toggle visibility of Waybar at runtime (e.g., via signals) if you configure your compositor appropriately.
4. **Multi-Bar / Multi-Output Setups**
   * For users with multiple monitors, you can define separate bars per output in the config; you might run into rendering challenges, but this is a supported use case.

---

## Troubleshooting

Some common issues and how to resolve them:

* **Configuration File Not Found**
  * Ensure that `~/.config/waybar/config` (or `.config/waybar/config.jsonc`) exists. If not, copy the system-wide config from `/etc/xdg/waybar/`.
* **Syntax Errors in JSONC**
  * Use a JSONC-aware editor or validator; missing commas or misplaced braces can prevent Waybar from starting.
* **Permission Issues**
  * Ensure the config files are owned by your user and readable; incorrect permissions can block loading.
* **Tooltips Not Showing**
  * Some users report that tooltips are unreliable unless certain GTK dependencies (like `gtk-layer-shell`) are correctly installed.
* **Multiple Bars Appear**
  * This may be due to misconfiguration in the compositor (e.g., using `status_command` vs `swaybar_command`).

---
