## `output`

### Definition

A **global bar configuration** option in Waybar that determines on which monitor(s) (Wayland outputs) a specific bar instance should appear.

---

### Data Type

* **Type:** `string` or `array`
* **Constraints:**
  * When a **string**, it specifies a single output identifier.
  * When an **array**, it can list multiple output identifiers, or use exclusion syntax with a leading `!`, or use a wildcard `*`.

---

### Semantics

* The `output` option configures which physical or logical monitor(s) your Waybar instance will be tied to.
* You can **target specific monitors** by their output names (e.g., `"HDMI-A-1"`) or by their descriptive strings as given by your compositor.
* The **exclamation mark (`!`)** prefix can be used to **exclude** certain outputs. For example, `"!DP-3"` means "all outputs except DP-3".
* The **wildcard `*`** can be used in arrays (typically at the end) to match *all remaining outputs*. This is useful especially when combined with exclusions.
* When you run **multiple Waybar instances**, you can define a separate `output` field in each instance (i.e., each bar configuration). This allows different bars on different monitors.
* If you **omit the `output` field**, the bar will appear on *all* outputs by default.

---

### Example

```json
{
  "output": "eDP-1",
}
```

```json
{
  "output": ["!eDP-1", "*"],
}
```

In this second example, Waybar will appear on **all outputs except** `eDP-1`.

---
