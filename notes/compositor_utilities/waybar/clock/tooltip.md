## `tooltip`

### Definition

The `tooltip` option is a module-level directive for the Waybar `clock` module. It controls whether a tooltip displaying the full date and/or time appears when the user hovers the mouse cursor over the clock module.

---

### Data Type

- **Type:** boolean  
- **Constraints:** Accepts only `true` or `false`.  
- Default behavior is generally `true`, meaning that a tooltip is shown unless explicitly disabled.

---

### Semantics

The `tooltip` option determines the visibility of the clock module’s hover tooltip:

- When set to `true`, hovering over the clock module triggers a popup displaying additional temporal information, typically the same value as defined by the `tooltip-format` option (or the `format` string if no tooltip-format is specified).  
- When set to `false`, the hover tooltip is suppressed, and no additional information appears on mouseover.  
- The tooltip mechanism relies on Waybar’s interaction hooks with the Wayland compositor and is therefore dependent on the environment supporting standard pointer events and tooltip rendering.  
- This option does not affect the regular visual output of the module in the bar; it only controls auxiliary hover behavior.

---

### Example

Here is a minimal valid configuration snippet showing typical usage of `tooltip`:

```json
{
  "clock": {
    "tooltip": true
  }
}
```

In this example, the tooltip is enabled, so moving the cursor over the clock module will display the configured date/time information in a popup.

---
