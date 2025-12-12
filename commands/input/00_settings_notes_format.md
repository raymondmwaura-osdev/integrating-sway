# <Setting Name>

## Overview

A concise description of the setting, its purpose, and the input devices it applies to.
List the device types this setting affects (e.g., keyboard, touchpad, mouse, all devices).

+ **Data Format**: Specify whether the setting accepts strings, numbers, keywords, booleans, ranges, or lists.
+ **Default Value**: State the default value applied if the setting is not explicitly configured.

---

## Valid Values

List all acceptable values, ranges, or options for the setting.

---

## Behavior and Effects

Describe precisely how this setting affects device behavior. Include:
- Immediate effect
- Startup behavior
- Runtime changes via `swaymsg`
- Edge cases or interactions with other settings

---

## Examples

### Terminal Example

```bash
# Example of applying this setting at runtime using swaymsg
swaymsg input <identifier> <setting> <value>
````

### Config File Example

```bash
# Example of applying this setting in the Sway configuration file
input "<identifier>" {
    <setting> <value>
}
```

---

## Notes

Any additional information, warnings, or practical observations for this setting.

---
