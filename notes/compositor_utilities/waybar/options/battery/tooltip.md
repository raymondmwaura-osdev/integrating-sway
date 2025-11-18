## `tooltip`

### Definition

A **module-level directive** in Waybar that controls whether a **hover tooltip** is displayed for the battery module. It provides additional battery information when the user hovers the mouse pointer over the module.

---

### Data Type

* **Type:** `boolean`
* **Default:** `true`

---

### Semantics

* When `tooltip` is **`true`**, Waybar generates a tooltip containing extended battery information, typically including:

  * Battery percentage
  * Status (`Charging`, `Discharging`, `Full`)
  * Estimated remaining time (if available)
  * Optional device name

* When `tooltip` is **`false`**, hovering over the battery module will **not** display any additional information.

* The tooltip content is automatically formatted by Waybar; users **cannot directly customize the text** through this option, but other module options (like `format`) control what is visible in the main bar.

* Tooltip display is dependent on **pointer support** in the compositor. In some Wayland setups without pointer events, tooltips may not appear.

---

### Example

```json
{
  "tooltip": false
}
```

---
