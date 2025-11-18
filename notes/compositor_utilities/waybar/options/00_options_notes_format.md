# "<option name>"

### Definition

A concise statement describing the option’s functional role within the Waybar configuration system and the specific layer to which it belongs (global bar configuration, module-level directive, or interaction hook).

---

### Data Type

Identify the expected data type (string, number, array, boolean, object) and any constraints (e.g., accepted enumerations, numerical bounds, structural requirements).

---

### Semantics

Describe the operational effect of the option on Waybar’s behavior. Clarify how Waybar interprets the value, how it influences rendering or module behavior, and whether it interacts with compositor-level features.

---

### Usage Context

Specify where the option is valid (e.g., “Applicable only within module blocks,” “Global bar-level parameter,” or “Exclusive to Sway-specific modules”). Include any dependencies or conditions under which the option is evaluated.

---

### Example

Provide one minimal, correct JSON example that demonstrates typical usage of the option. The example should highlight only the option being documented to preserve focus.

```json
{
  "example-key": "example-value"
}
```

---

### Interactions and Caveats

Document noteworthy interactions with other options, operational side effects, and implementation nuances (e.g., update timing, compositor compatibility, implicit defaults, or potential failure modes).

---
