The two types of dropdown controls commonly encountered in ASP.NET / Telerik applications like RepairData are the **Telerik RadComboBox** and the **Telerik RadDropDownList** (or standard HTML <select> / ASP.NET DropDownList).

1. Telerik RadComboBox

- **What it is:** A composite AJAX text-input and dropdown control.
- **DOM Structure:** It consists of a visible editable textbox (_textbox), an arrow button (_span), and a pop-up list containing individual items (_listItem).
- **Behavior:** It allows users to type text directly into the field to filter large datasets in real time before making a selection.
- **Where it's used:** Searchable or high-volume fields like Item Numbers, Warehouses, or Customer accounts.

2. Telerik RadDropDownList (or Standard <select>)

- **What it is:** A non-editable, fixed-option selection menu.
- **DOM Structure:** It renders as a standard HTML <select> element containing <option> tags, or a lightweight non-editable Telerik wrapper.
- **Behavior:** Users cannot type freeform text into the box to filter; they click or scroll to select from a fixed list.
- **Where it's used:** Short, predictable option lists like Status, Yes/No, or Security Templates.

---

Why You Need to Know the Difference in SAHI Automation

Knowing which control is on the screen determines which SAHI API and interaction sequence you must use:

- **API Compatibility:** Passing a **RadComboBox** to _setSelected() will fail because SAHI cannot find a native <select> tag in the DOM. Conversely, trying to target a _textbox on a standard **RadDropDownList** will fail because no input text element exists.
- **ViewState & Event Lifecycle:** RadComboBoxes rely on a series of client-side JavaScript events (onfocus, onchange, onblur) to copy the selected item into ASP.NET's hidden ViewState form field. Standard <select> elements commit their selection immediately upon interaction.

---

Code Comparison

**Recommended handling for a Standard Dropdown / RadDropDownList:**

_setSelected(_select("ctl00$mc$ddlStatus"), "Active");

**Recommended handling for a RadComboBox:**

var $txtInput = _textbox("ctl00_mc_ddlItemNumber_Input");

var $btnArrow = _span("ctl00_mc_ddlItemNumber_Arrow");

_focus($txtInput);

_setValue($txtInput, "00A");

_click($btnArrow);

_click(_listItem("00A-KIT-00"));

_blur($txtInput);

How to identify the type

You can easily identify which type of dropdown control you are looking at using **three different methods**: SAHI Controller, Chrome DevTools, or Visual Behavior.

---

Method 1: The SAHI Controller (Fastest Method)

Hold **CTRL** and hover your mouse over the control while SAHI Controller is open:

- **Standard Dropdown:** The **Accessor** field displays **_select(...)**.

- Example: _select("ctl00$mc$DropDownList1")

- **RadComboBox:** The **Accessor** field displays a **_textbox(...)** when hovering over the text area, and a **_span(...)** or **_image(...)** when hovering over the arrow button.

- Example (Text Area): _textbox("ctl00_mc_ddlItemNumber_Input")
- Example (Arrow Button): _span("ctl00_mc_ddlItemNumber_Arrow")

---

Method 2: Inspect Element in Browser (Chrome DevTools - F12)

Right-click the dropdown on screen and click **Inspect**:

- **Standard Dropdown:** You will see a native HTML **<select>** tag containing **<option>** child elements in the DOM:

<select name="ctl00$mc$DropDownList1" id="ctl00_mc_DropDownList1">

    <option value="Active">Active</option>

</select>

- **RadComboBox:** You will see a composite wrapper containing an **<input type="text">** element paired with a button/arrow element:

<div id="ctl00_mc_ddlItemNumber" class="RadComboBox">

    <input id="ctl00_mc_ddlItemNumber_Input" type="text" />

    <span id="ctl00_mc_ddlItemNumber_Arrow"></span>

</div>

---

Method 3: Visual & Keyboard Behavior

- **Standard Dropdown:** You cannot type freeform text inside the box to filter list items dynamically. Clicking it opens a standard browser menu.
- **RadComboBox:** You can click inside the box, see a blinking text cursor, and **type letters to filter the list in real time**.

---

Quick Reference Summary

|   |   |   |
|---|---|---|
|Dropdown Type|SAHI Accessor|Automation API / Approach|
|**Standard Dropdown**|_select(...)|**1 line:** _setSelected(_select("..."), "Value");|
|**RadComboBox**|_textbox(...) + _span(...)|**4-step sequence / useKeypress_dropDown_v5 helper**|