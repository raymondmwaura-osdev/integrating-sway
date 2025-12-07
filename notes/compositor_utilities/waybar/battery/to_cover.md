* **`bat`** – `string` – Specifies which battery to monitor (e.g., `/sys/class/power_supply/BAT0`). Auto-detected if not set.
* **`adapter`** – `string` – Specifies which power adapter to monitor. Auto-detected if not set.
* **`design-capacity`** – `bool` – If `true`, shows battery percentage relative to original design capacity instead of current max.
* **`full-at`** – `integer` – Defines a custom “full” percentage for batteries with charge limits (e.g., 90%).
* **`states`** – `object` – Define named thresholds like `warning` or `critical` for CSS styling.
* **`events`** – `object` – Run commands on state transitions (e.g., `on-discharging-warning`).
* **`format-time`** – `string` – Time estimate format (time to full/empty), using `{H}`, `{M}`, `{m}`.
* **Custom formats** – Override formats depending on **state**, **status**, or both (`format-<state>`, `format-<status>`).
* **`max-length`** – `integer` – Maximum number of characters for the module’s label.
* **`min-length`** – `integer` – Minimum width in characters for the module.
* **`align`** – `float` – Alignment of text in module (0 = left, 1 = right).
* **`justify`** – `string` – Text alignment inside the label: `left`, `right`, or `center`.
* **`rotate`** – `integer` – Rotate text in 90° increments (0, 90, 180, 270).
* **`on-click`** – `string` – Command to run when clicking the battery module.
* **`tooltip`** – `bool` – Show a tooltip on hover (default: `true`).
* **`tooltip-format`** – `string` – Format for tooltip; supports placeholders like `{capacity}`, `{power}`, `{time}`.
* **State mechanics:** CSS classes like `#battery.warning` or `#battery.critical` are applied when thresholds in `states` are reached.
