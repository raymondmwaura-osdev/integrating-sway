## `position`

### Definition

Within Waybar’s **global bar configuration**, the `position` option determines on which edge of the screen the bar is displayed (e.g., top, bottom, left or right).

---

### Data Type

* **Type:** `string`
* **Accepted values (enumeration):** `"top"`, `"bottom"`, `"left"`, or `"right"`
* **Default:** `"top"`.

---

### Semantics

The `position` field tells Waybar where to render the bar in relation to the screen’s edges. When set to:

* **`"top"`**; the bar is anchored along the top of the screen.
* **`"bottom"`**; the bar appears along the bottom edge.
* **`"left"`**; the bar is vertical, aligned to the left side. In this orientation, modules may need support for rotation (e.g., via the module-level `rotate` setting) so that their content reads correctly.
* **`"right"`**; similar to `"left"`, but aligns the bar vertically on the right side.

Waybar uses this value when initializing the bar window, which interacts with compositor-level window placement. The bar’s layout (height or width) may also depend on this setting (e.g., a vertical bar on the left/right may use width instead of height for sizing).

---

### Example

```json
{
  "position": "bottom"
}
```

---
