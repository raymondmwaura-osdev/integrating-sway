# Swaylock

## Introduction

**swaylock** is a screen‑locking utility designed for Wayland compositors. It is particularly well-suited for use with **Sway**, a tiling Wayland compositor, but it also works with any Wayland compositor that implements the `ext-session-lock-v1` protocol.

The purpose of swaylock is to secure the user’s session by obscuring the screen and requiring authentication (e.g., password via PAM) to unlock.

---

## Installation

1. **From Distribution Packages**
   Most Linux distributions include swaylock in their repositories. You can usually install it with your package manager:

   ```bash
   sudo pacman -S swaylock
   # or
   sudo apt update
   sudo apt install swaylock
   ```

2. **From Source**
   If you prefer to compile swaylock from source, you can clone the GitHub repository and build with Meson and Ninja:

   ```bash
   git clone https://github.com/swaywm/swaylock.git  
   cd swaylock  
   meson build  
   ninja -C build  
   sudo ninja -C build install  
   ```

   Dependencies include `wayland`, `wayland-protocols`, `libxkbcommon`, `cairo`, and optionally `gdk-pixbuf2` if you want background image support.

   If your system uses `/etc/shadow` for passwords and does not use PAM, you may need to set the **suid** bit on the installed binary so that swaylock can read shadow passwords:

   ```bash
   sudo chmod a+s /usr/local/bin/swaylock  
   ```

---

## Basic Usage

Once installed, swaylock can be invoked from a terminal or bound to a key within Sway. Key usage options include:

* Run `swaylock` with no arguments to lock the screen using default settings.
* Customize the background color with `--color RRGGBB`, where `RRGGBB` is a hexadecimal color code.
* Display a background image using `--image /path/to/image`.
* Tile the background image across multiple monitors with `--tiling`.
* Disable the visual unlock indicator with `--no-unlock-indicator`.
* Show the number of failed authentication attempts using `--show-failed-attempts`.
* Run swaylock as a background process using `--daemonize`.
* Specify a custom configuration file with `--config /path/to/config` for persistent settings.

These options provide flexibility for both aesthetic customization and secure session management.

---

## Configuration

While you can pass many options via command line, **persistent configuration** is better handled via a config file.

1. **Default Locations**

   * `~/.config/swaylock/config`
   * or under `$XDG_CONFIG_HOME/swaylock/config`

2. **Configuration Format**
   The config file supports key-value pairs for customizing:

   * Background (color or image)
   * Text color
   * Font and font size
   * Unlock indicator (on/off)
   * Other UI parameters

3. **Integrating with Sway Idle Management**
   To automatically lock the screen after inactivity, you typically use **swayidle**, the idle management daemon.

   Example snippet in `~/.config/sway/config`:

   ```text
   exec swayidle -w \
     timeout 1800 'swaylock -f' \
     timeout 1810 'swaymsg "output * power off"' \
     resume 'swaymsg "output * power on"' \
     before-sleep 'swaylock -f'
   ```

   * `timeout 1800`: after 30 minutes of idle, run `swaylock -f` (daemonized)
   * `before-sleep`: ensures screen is locked before system suspends.
   * `resume`: turns outputs back on when activity is detected.

4. **Key Binding for Manual Lock**
   In your Sway config, bind a key to lock manually, for example:

   ```
   bindsym $mod+Shift+l exec swaylock -f
   ```

   Here, `$mod` is typically the "Super" (Windows) key.

---

## Security Considerations

* **PAM Integration**: swaylock relies on PAM (Pluggable Authentication Modules) for authentication. Make sure your `/etc/pam.d/swaylock` (or relevant PAM config) is correctly set up.
* **SUID Permissions**: If not using PAM and relying on shadow passwords, ensure the binary permissions are correctly set (suid or sgid) so it can read `/etc/shadow`.
* **Multi‑Monitor Behavior**: When using images, background behavior across multiple outputs may need careful configuration (tiling, relative image positioning, etc.).
* **Attack Surface**: swaylock is designed to be minimal to reduce its attack surface; avoid adding unnecessary complexity in your lock screen.

---

## Advanced Customizations

* **Theming**: You can integrate more elaborate themes by using images, custom fonts, colors, and indicator styles in your `swaylock` configuration.
* **Effect Variants / Forks**: There are forks such as **swaylock-effects** that support additional visual effects (blur, fade-in, clock display). (Note: using such forks may change how you configure or invoke swaylock.)
* **Suspend / Resume Behavior**: Because of timing issues when suspending and locking, people often add delays or use `--daemonize` to ensure swaylock runs properly before system sleep.
* **YubiKey / 2FA**: It is possible to configure PAM (and thus swaylock) to use U2F (e.g., YubiKey) for unlocking.

---

## Troubleshooting

1. **Screen not turning on after resume**
   After resuming, the screen may remain off unless swaylock was run with `-f` (daemonized), because swayidle waits for the lock process to end.

2. **Unlocking delay or failure**

   * If you see “WRONG” immediately even with correct password: check PAM config.
   * Lag in unlock or graphical glitches: might be related to delay in snapshotting or redrawing background (especially with effects).

3. **Lock screen artifacts on resume**
   If upon resume you briefly see the desktop content before swaylock engages, you may need to delay suspend or adjust the `before-sleep` command in swayidle.

4. **Cursor or pointer issues**
   In some environments, the pointer may not behave as expected; you can control pointer appearance with the `--pointer` option.

---

## Example Complete Setup (Minimal)

Here is a minimal but secure setup for Sway + swaylock + swayidle, in **`~/.config/sway/config`**:

```
# Variables
set $mod Mod4
set $locker swaylock -f   # run swaylock in daemon mode

# Key binding for manual lock
bindsym $mod+Shift+l exec $locker

# Idle behaviour
exec swayidle -w \
  timeout 1200 '$locker' \         # lock after 20 minutes
  timeout 1210 'swaymsg "output * dpms off"' \  
  resume 'swaymsg "output * dpms on"' \
  before-sleep '$locker'

# (other sway config …)
```

And a simple `~/.config/swaylock/config`:

```
# Example swaylock config
color = "1a1a1a"          # dark gray background
text-color = "ffffff"     # white text
font = "monospace"
font-size = 24
indicator-thickness = 4
```

---
