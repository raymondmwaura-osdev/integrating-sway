# `<command>`

## Overview

A concise description of the command, its primary function, and context of use within Sway. Include whether it is primarily for **window management**, **workspace control**, **system execution**, or **layout manipulation**.

---

## Syntax

Provide the exact command syntax, including optional arguments and placeholders. Example:

```
swaymsg move container to workspace <workspace-number>
```

---

## Parameters / Arguments

List and explain each parameter or argument in detail:

+ `Parameter`: Description. Example.

---

## Behavior and Effects

Describe precisely what the command does when executed. Include edge cases, defaults, or special conditions.

---

## Usage Examples

Provide at least **two practical examples** showing how the command can be used, both in the terminal and in the Sway configuration file.

**Terminal Example:**

```bash
swaymsg move container to workspace 2
```

**Config File Example:**

```bash
bindsym $mod+2 move container to workspace 2
```

---

## Related Commands

List any commands that are similar or commonly used in conjunction with this command.

---
