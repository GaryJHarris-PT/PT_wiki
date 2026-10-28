**No, you do not need to call** **_blur()** **after clicking a button** like `_span("Receive")` or `_span("Create")`.

`_blur()` is specifically designed for **input fields** (like `_textbox` or combo boxes) to force the browser to fire `onblur` / `onchange` events and write the text into client memory before submitting.

---

What You Need After Clicking a Button: Dynamic Waits

When you click buttons like `_span("Receive")` or `_span("Create")`, ASP.NET / Telerik applications trigger an **AJAX postback or page transition**.

Instead of blurring the button, what you need after the `_click()` is a **dynamic wait** to let the server process the action and render the updated screen:

```
// 1. Click the action button
_click(_span("Receive"));

// 2. Wait dynamically for the next screen/message element to appear
_wait(5000, _isVisible(_span("ctl00_mc_lblSuccessMessage")));

// 3. Assert the result
_assertEqual("Stock Received Successfully", _getText(_span("ctl00_mc_lblSuccessMessage")));
```

---

Two Golden Rules for Buttons & Postbacks

1. **_blur()** **BEFORE clicking (on input fields):** Always blur the last `_textbox()` or dropdown _before_ clicking the button if ASP.NET hasn't registered the final value:

```
_setValue(_textbox("ctl00_mc_txtQty"), "10");
_blur(_textbox("ctl00_mc_txtQty")); // Commit field value first
_click(_span("Receive"));            // Then trigger submit
```

1. **_wait()** **AFTER clicking (for postbacks):** Wait for the loader to disappear or the target header/success message on the next screen to become visible before executing subsequent lines.