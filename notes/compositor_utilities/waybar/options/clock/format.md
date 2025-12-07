## "format"

### Definition

The `format` option is a module-level directive for the Waybar `clock` module. It determines how Waybar renders the date/time display for that module (i.e. what textual representation is shown in the bar under “clock”).

---

### Data Type

- **Type:** string  
- **Constraints:** The string must follow the formatting syntax accepted by Waybar’s underlying date/time library; essentially the formatting specifiers inspired by `strftime` / the `date.h` (C++/chrono) formatting conventions.
- The default value, when unspecified, is `"{:%H:%M}"`.

---

### Semantics

When Waybar renders the `clock` module, it interprets the `format` string to produce the visible label showing current date and/or time. The placeholder syntax works as follows:

- The outer braces (e.g. `"{:…}"`) denote that what is inside should be processed as a formatting directive rather than literal text.
- Within the braces, standard date/time formatting specifiers dictate what components of date/time to show (hours, minutes, seconds, day, month, timezone, etc.), using conventions from the library cited in Waybar’s docs.
- Any literal text outside or around the braces (e.g. a clock icon, separator, extra spaces) is rendered as-is.  
- On each update interval (controlled by the `interval` option), Waybar re-evaluates the `format` string to reflect the current time/date.
- If the format string is invalid (e.g. uses unsupported specifiers), Waybar may log an error and omit rendering the module.
- Because `format` is a module-level directive, it only affects that instance of the `clock` module, not global bar layout or other modules.  

---

### Example

Here is a minimal valid configuration snippet showing typical use of `format`:

```json
{
  "clock": {
    "format": "{:%Y-%m-%d %H:%M:%S}"
  }
}
````

In this example:

* `%Y` → full year,
* `%m` → zero-padded month number,
* `%d` → zero-padded day of month,
* `%H` → hour in 24-hour clock,
* `%M` → minute,
* `%S` → second.

Thus, the clock module will show something like: `2025-12-07 14:32:05`.

---
