## `format-icons`

### Definition

A **module-level directive** in Waybar that defines **custom icons for different battery states**. It belongs specifically to the `battery` module and allows users to replace the default icons with user-defined symbols for various charge levels and statuses.

---

### Data Type

* **Type:** `object`
* **Structure:** Key-value pairs, where the key is a **status identifier** or **charge level range**, and the value is a string representing the icon.
* **Accepted keys:**

  * `"charging"`; used when the battery is charging
  * `"discharging"`; used when the battery is discharging
  * `"full"`; used when the battery is fully charged
  * `"empty"`; used when the battery is critically low
  * `"unknown"`; used when the status cannot be determined
  * Optional numeric keys can represent thresholds (e.g., `"20"`, `"50"`, `"80"`) to show specific icons at certain battery percentages.

---

### Semantics

* `format-icons` maps **battery states or percentage thresholds** to icons that are displayed in place of the default battery symbols.
* Waybar checks the battery’s **current status and percentage** on each update and selects the corresponding icon according to the `format-icons` mapping.
* This allows a **fully customized visual representation** of battery levels, supporting fonts like Font Awesome, Material Icons, or custom glyphs.
* If a given status or threshold is not defined, Waybar falls back to the **default icons**.
* Works in conjunction with the `format` option; the `{icon}` placeholder in `format` is replaced using the mapping defined in `format-icons`.

---

### Example

Minimal JSON example:

```json
{
  "format-icons": {
    "charging": "🔌",
    "full": "🔋",
    "discharging": "⚡",
    "empty": "❗",
    "unknown": "❓"
  }
}
```

---
