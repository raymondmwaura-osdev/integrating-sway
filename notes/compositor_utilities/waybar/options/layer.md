## `layer`

### Definition

This is a **global bar configuration** option in Waybar. It determines whether the bar is rendered *in front of* (`"top"`) or *behind* (`"bottom"`) other application windows.

---

### Data Type

- **String**  
- Accepted values (enumeration): `"top"`, `"bottom"`
- Default: `"bottom"`

---

### Semantics

- When set to `"bottom"`, Waybar will draw the bar **behind** application windows. This makes the bar less obtrusive but may hide parts of it if other windows cover that area.
- When set to `"top"`, Waybar draws the bar **above** other application windows. This ensures that the bar is always visible, even when other windows overlap.
- The `layer` option influences how the Wayland compositor arranges Waybar in relation to other surfaces. For compositors supporting *layer-shell* (e.g., via GTK layer shell), using `"top"` may be necessary to ensure Waybar popups or menus are not occluded.
- It does **not** directly affect module behavior (such as how modules draw or update), but rather the *Z-order* (stacking order) of the bar itself.

---

### Example

```json
{
  "layer": "top"
}
```

---
