
See also [[Dynamic Wait]] 

In SAHI Pro, **`_focus()`** programmatically sets browser keyboard/DOM focus onto a specific input element (like a textbox, combo box, or search field).

While `_blur()` tells the browser _"the user left this field"_, **`_focus()`** tells the browser _"the user just clicked into or tabbed into this field"_.

---

### Why `_blur()` Alone Can Fail in ASP.NET / Telerik

In complex web frameworks like ASP.NET AJAX or Telerik RadComboBoxes, client-side JavaScript event handlers listen for a full event lifecycle:

\[\text{`onfocus`} \longrightarrow \text{`onchange` / `oninput`} \longrightarrow \text{`onblur`}\]

If SAHI executes `_setValue()` followed immediately by `_blur()`, the browser sets the text, but **the field never recorded an initial `onfocus` event**. As a result:

1. The ASP.NET client-side script doesn't realize the user actually entered the field.
2. The `onblur` event handler fires on an "unfocused" field, so it skips validation or fails to sync the hidden ViewState field.

---

### The Complete Event Sequence (`_focus` + `_setValue` + `_blur`)

When a stubborn field or dropdown is still throwing validation errors (like _"Level code required"_ or losing its value on postback), sandwich your input between `_focus` and `_blur`:

```
var $txtLevelInput = _textbox("ctl00_mc_ddlLevelCode_Input");

// 1. Focus the field so ASP.NET initializes client tracking
_focus($txtLevelInput);

// 2. Set the value
_setValue($txtLevelInput, "M");

// 3. Open dropdown arrow & pick list item (for Telerik combo boxes)
_click(_span("ctl00_mc_ddlLevelCode_Arrow"));
_click(_listItem("M - Maintenance"));

// 4. Fire blur to trigger validation/postback
_blur($txtLevelInput);
```

---

### 3 Other Reasons `_blur` Might Still Fail (And How to Fix Them)

If adding `_focus()` still leaves you with an error, check these three common ASP.NET quirks:

#### 1. Simulating an Enter Key Press

Some Telerik grid filters and combo boxes require an explicit `ENTER` key press rather than a `_blur()` event to commit the selection:

```
_setValue($txtFilterItemNum, "00A-KIT-00");
_keyPress($txtFilterItemNum,); // 13 is the ASCII code for Enter
```

#### 2. Targeting the Hidden Field vs. The Visible Input

Telerik dropdowns often render two elements in the HTML DOM:

- A hidden `<select>` or `<input>` used for form submission.
- A visible `<input>` styled with `_Input` in its ID for typing.

Make sure your `_focus()`, `_setValue()`, and `_blur()` are targeted strictly at the **visible `_Input` textbox** (e.g., `ctl00_mc_ddlLevelCode_Input`).

#### 3. AJAX Postback Timing (`_wait`)

If the field triggers an automatic AJAX panel refresh when blurred, SAHI might be trying to click the next button while the DOM is re-rendering. Add a quick dynamic wait right after `_blur()`:

```
_blur($txtInput);
_wait(3000, !_isVisible(_div("ctl00_mc_RadAjaxLoadingPanel"))); // Wait for loader to disappear
```

---

💡 Which specific field or screen is giving you the error right now? If you share the field name or control ID, we can tailor the exact 3-line sequence for it!