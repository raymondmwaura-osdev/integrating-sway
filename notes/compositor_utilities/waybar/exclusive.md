## `exclusive`

### Definition

A **global bar configuration** option in Waybar that controls whether Waybar requests an *exclusive zone* from the compositor.

---

### Data Type

* **Type:** `boolean`
* **Default:** `true`

---

### Semantics

* When `exclusive` is set to **`true`**, Waybar asks the compositor to reserve a zone (on the specified screen edge) that other application windows **cannot** overlap. This ensures that the bar does not cover or get covered by application windows.
* When `exclusive` is **`false`**, the bar does *not* request this reserved zone. That means other windows may **draw under or on top of** the bar, depending on the compositor’s behavior.
* The implementation in Waybar’s code explicitly uses this option to set the surface’s exclusive‑zone behavior: `surface_impl_->setExclusiveZone(exclusive);`
* The `exclusive` flag interacts with other settings like `passthrough`: if you disable the exclusive zone, you might combine that with `passthrough = true` to make the bar more “transparent” to pointer events.

---

### Example

```json
{
  "exclusive": false
}
```

---
