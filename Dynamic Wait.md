See also [[_blur and _focus]] 

In SAHI Pro, the basic syntax for a **dynamic wait** is:

```
_wait($timeoutInMs, $condition);
```

---

Key Components

- **$timeoutInMs**: The maximum time (in milliseconds) SAHI will pause before giving up (e.g., `5000` for 5 seconds).
- **$condition**: A boolean expression or SAHI locator check that evaluates to `true` when ready.

---

Common Examples

1. Wait for an element to become visible (Most Common)

```
_wait(5000, _isVisible(_span("ctl00_pageName")));
```

2. Wait for an element to exist in the DOM

```
_wait(5000, _exists(_textbox("ctl00_mc_txtItemNumber")));
```

3. Combined condition (Visibility + Specific Text)

```
_wait(5000, _isVisible($spanHeader) && _getText($spanHeader) === "Receive Stock");
```

---

Best Practice Reminder

Always **separate the** **_wait** **from your** **_assertEqual** **assertion**. This ensures SAHI logs show exact expected vs. actual values if the page load fails, rather than a generic timeout error:

```
// 1. Dynamic wait until the header element is visible
_wait(5000, _isVisible(_span("ctl00_pageName")));

// 2. Explicit assertion for clean log feedback
_assertEqual("Receive Stock", _getText(_span("ctl00_pageName")));
```