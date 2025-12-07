## `modules-center`

### Definition

The `modules-center` option is a global bar-level configuration directive in Waybar. It specifies an ordered list of modules that will be rendered in the center section of the bar, between `modules-left` and `modules-right`.

---

### Data Type

- **Type:** array of strings  
- **Constraints:** Each string must correspond to the `name` of a defined module in the configuration file. The order of items in the array determines the left-to-right display order within the center section. Duplicates are allowed but will render multiple instances of the module.  

---

### Semantics

The `modules-center` option dictates the placement and rendering sequence of modules in the bar’s central region:

- Waybar first allocates space for `modules-left` and `modules-right`, then positions `modules-center` modules in the remaining central area.  
- The modules are displayed horizontally in the order listed.  
- If a module specified in `modules-center` does not exist in the configuration, Waybar ignores it and logs a warning.  
- This option interacts with module alignment and bar sizing but does not affect individual module behavior beyond placement.  
- Empty arrays (`[]`) are valid and result in no modules being displayed in the center.  
- `modules-center` is mutually independent of `modules-left` and `modules-right`, allowing flexible bar customization.  

---

### Example

Here is a minimal valid configuration snippet showing typical usage of `modules-center`:

```json
{
  "modules-center": ["clock", "network", "custom-weather"]
}
````

In this example, the `clock`, `network`, and `custom-weather` modules will appear in the center of the bar, arranged from left to right in that order.

---
