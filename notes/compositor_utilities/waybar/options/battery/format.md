## `format`

### Definition

A **module-level directive** in Waybar that defines the **string template** used to display the battery’s status in the bar. It belongs to the `battery` module configuration.

---

### Data Type

* **Type:** `string`
* **Constraints:** Can include **placeholders** enclosed in curly braces `{}` which are replaced with dynamic battery information at runtime. Common placeholders include:

  * `{capacity}`; current battery percentage (integer)
  * `{icon}`; battery icon reflecting charge state
  * `{status}`; textual state: `Charging`, `Discharging`, `Full`, `Unknown`
  * `{time}`; estimated remaining time (if available)

* Any additional text outside placeholders is rendered literally.

---

### Semantics

* The `format` string determines the **visual presentation** of the battery module in Waybar.
* Waybar **replaces each placeholder** with real-time battery data on each update (defined by the `interval` option).
* Supports **custom text and symbols**, allowing users to prepend or append labels, e.g., `"Battery: {capacity}%"`.
* The module automatically adjusts **icon display** based on charge state and capacity if `{icon}` is used.
* Incorrect placeholders (not recognized by Waybar) are rendered as literal text.

---

### Example

```json
{
  "battery": {
    "format": "{icon} {capacity}% ({status})",
  }
}
```

This configuration shows the battery icon, percentage, and textual state.

---
