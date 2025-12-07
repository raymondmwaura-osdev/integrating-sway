## `interval`

### Definition

The `interval` option is a module-level directive for the Waybar `clock` module. It specifies the frequency, in seconds, at which Waybar updates the displayed time/date for that module.

---

### Data Type

- **Type:** number (integer or floating-point)  
- **Constraints:** Must be a positive number. Values less than 1 are technically allowed but may lead to unnecessary CPU usage. Zero or negative values are invalid and typically cause Waybar to default to a safe interval (commonly 60 seconds).

---

### Semantics

The `interval` option controls the update cadence of the `clock` module:

- Waybar uses this value to schedule redraws of the module.  
- The module queries the system clock and updates its output on the bar according to this interval.  
- Shorter intervals (e.g., 1 second) allow for displaying seconds in real time, while longer intervals (e.g., 60 seconds) reduce CPU usage for minute-only updates.  
- This option interacts indirectly with rendering performance: excessively small intervals may increase CPU load, especially with multiple modules updating frequently.  
- If `interval` is not explicitly set, the module uses a default of 60 seconds for minute-level updates, unless the `format` string includes seconds, in which case Waybar automatically reduces the interval to 1 second.

---

### Example

Here is a minimal valid configuration snippet showing typical usage of `interval`:

```json
{
  "clock": {
    "interval": 1
  }
}
````

In this example, the clock module updates every second, which is suitable for a format string displaying hours, minutes, and seconds.

---
