## `height`

### Definition

A **global bar configuration** option in Waybar that specifies the vertical thickness of the bar when displayed along horizontal edges (`top` or `bottom`). For vertical bars (`left` or `right`), `width` is used instead.

---

### Data Type

* **Type:** `number`
* **Units:** Pixels (integer values)
* **Constraints:** Must be a positive integer. Some compositors may enforce a minimum or maximum size.

---

### Semantics

* The `height` option directly sets the number of pixels Waybar reserves for the bar along the vertical axis.
* It affects the **exclusive zone** requested from the compositor if `exclusive` is `true`. A taller bar will push application windows further away from the screen edge.
* The bar’s visual elements (modules, icons, and text) scale within this height. If the height is too small, content may be truncated; if too large, the bar may occupy excessive screen space.
* For vertical bars (`left`/`right`), Waybar ignores `height` in favor of `width`.

---

### Example

```json
{
  "height": 32
}
```

---
