## `modules-right`

### Definition

A **global bar configuration** option in Waybar that specifies which modules are displayed on the **right-hand side** of the bar. It defines the order and presence of modules for a given bar instance.

---

### Data Type

* **Type:** `array of strings`
* **Constraints:** Each element must correspond to a **valid module name** defined in Waybar, such as `"clock"`, `"battery"`, `"network"`, or any custom module. The order in the array determines the left-to-right rendering of the modules on the right side of the bar.

---

### Semantics

* `modules-right` controls **layout and composition** of the bar’s right section.
* Modules listed here are rendered **horizontally** (or vertically, if the bar itself is vertical) in the specified order.
* If a module listed in `modules-right` is **not defined** elsewhere in the configuration (i.e., module-specific settings are missing), Waybar will still attempt to render it using default settings.
* This option interacts with **`modules-left`**, which controls the left-hand section, and **`modules-center`**, which controls the center. Waybar automatically spaces these sections according to the bar’s total width and alignment rules.
* Omitting this option results in **no modules displayed** on the right-hand side.

---

### Example

Minimal JSON example:

```json
{
  "modules-right": ["clock", "battery"]
}
```

---
