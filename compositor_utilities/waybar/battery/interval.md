## `interval`

### Definition

A **module-level directive** in Waybar that specifies the polling frequency for the `battery` module, controlling how often battery status information is refreshed.

---

### Data Type

* **Type:** `number`
* **Units:** Seconds
* **Constraints:** Must be a positive number. Non-integer values are permitted, allowing sub-second precision if the underlying system supports it.

---

### Semantics

* The `interval` option defines the **update frequency** for the battery module. Waybar will query the system’s battery status (charge level, charging state, etc.) at intervals specified by this value.
* A **smaller interval** (e.g., 5 seconds) results in more frequent updates, providing near-real-time battery information, but increases CPU usage slightly.
* A **larger interval** (e.g., 60 seconds) reduces CPU overhead but may display slightly outdated battery information.
* This option interacts with system events only in a **polling manner**; it does not automatically subscribe to kernel power events, so the interval determines how quickly changes are reflected in the UI.

---

### Example

```json
{
  "interval": 30
}
```

Contextual usage within the `battery` module:

```json
{
  "battery": {
    "interval": 15
  }
}
```

This configuration updates the battery module every 15 seconds, displaying the remaining capacity as a percentage.

---
