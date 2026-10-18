# SAHI Pro Test Framework - Naming Conventions & Standards

This document serves as the single source of truth for coding standards, variable scoping conventions, and UI element interaction patterns across the SAHI Pro test automation repository.

---

## 1. Variable Scope & Naming Conventions

To maintain readability, avoid variable shadowing, and clarify variable lifetime at a glance, follow these strict prefix conventions:

|Scope|Prefix|Description|Examples|
|:--|:--|:--|:--|
|**Global Config**|`$CFG_`|System-wide environment settings, base URLs, and global configurations|`$CFG_BaseUrl`, `$CFG_BrowserType`, `$CFG_Timeout_MS`|
|**Module Libraries**|`$ORD_`, `$CUST_`, `$PROD_`|Shared default data and library constants categorized by functional domain|`$ORD_DefaultStatus`, `$CUST_DefaultRegion`, `$PROD_DefaultTaxCode`|
|**Script-Level**|`$s_`|Variables declared in the main body of an individual `.sah` test script|`$s_OrderNumber`, `$s_CustomerID`|
|**Function Parameters**|`$p_`|Parameters passed into a function signature|`$p_AccountType`, `$p_StatusFilter`, `$p_Category_listItem`|
|**Function Locals**|`$local`|Temporary variables declared and scoped within a function body|`$localCategoryAbbr`, `$localSelectedStatus`|

---

## 2. Examples in Practice

### Function Signatures & Local Scope

```
// Function parameters use $p_, local variables use $local
function createCustomerAccount($p_AccountType, $p_Category_listItem, $p_InitialStatus, $p_Region_listItem) {

    // Local variable prefix
    var $localCategoryAbbr = $p_Category_listItem.substr(0, 3);

    _setValue(_textbox("txtCategoryCode_Input"), $localCategoryAbbr);
    _click(_span("cmbCategoryCode_Arrow"));
    _click(_listItem($p_Category_listItem));

    _blur(_textbox("txtCategoryCode_Input"));
    _wait(1000);

    var $localSelectedStatus = _getText(_textbox("ddlCurrentStatus_Input"));

    if ($localSelectedStatus == "Please Select") {
        _click(_span("cmbCurrentStatus_Arrow"));
        _click(_listItem($p_Region_listItem));
        _blur(_textbox("ddlCurrentStatus_Input"));
    }
}
```

### Script Execution & Module Library Call

```
// In main test script (e.g., TC01_Create_Account.sah)
var $s_OrderNumber = "ORD-2026-001";
var $s_CustomerID = "CUST-90821-X";

// Calling helper function with script-level variables and domain constants
assignCustomerToRegion($CUST_DefaultRegion, $s_CustomerID, $s_OrderNumber);
```

---

## 3. UI Control & Event Handling Rules (ASP.NET / Telerik & Dynamic UIs)

When interacting with ASP.NET AJAX, Telerik RadComboBox, and dynamic JavaScript controls, standard clicks alone may not trigger underlying change events. Follow these mandatory practices:

1. **Dropdown Arrow Accessor:**
    
    - Always use `_span` (or `_image` if applicable) for dropdown arrows, never `_link`.
    - _Example:_ `_click(_span("cmbAccountType_Arrow"));`
2. **Hidden Field Commitment:**
    
    - Always trigger `_blur()` or type/set the value prior to selecting from the list to ensure the hidden form input commits the selected value before step transitions or saves.
    - _Example:_
        
        ```
        _setValue(_textbox("txtCategoryCode_Input"), $localCategoryAbbr);
        _click(_span("cmbCategoryCode_Arrow"));
        _click(_listItem($p_Category_listItem));
        _blur(_textbox("txtCategoryCode_Input"));
        ```
        
3. **Avoid Consecutive Clicks on Dropdown Arrows:**
    
    - Single-click dropdown arrows. Consecutive clicks close the dropdown immediately, preventing SAHI from selecting the list item.
4. **Dynamic Waiting for Postbacks:**
    
    - Use dynamic waits over fixed sleeps for AJAX grid rendering and asynchronous updates:
        
        ```
        _wait(5000, _isVisible(_span("gridOrders_ctl00_ddlStatus_Arrow")));
        ```
        

---

## 4. Repository File & Folder Architecture

Organize files systematically at the root directory:

```
/SAHI_Scripts_Collection/
│
├── NAMING_CONVENTIONS.md        <-- Framework Coding Standards & Conventions
├── README.md                    <-- Repository Overview & Execution Setup
│
├── /libs/                       <-- Shared Function & Module Libraries
│   ├── global_lib.sah           <-- Framework setup & environment constants ($CFG_)
│   ├── orders_lib.sah           <-- Order domain functions & constants ($ORD_)
│   ├── customers_lib.sah        <-- Customer domain functions & constants ($CUST_)
│   └── products_lib.sah         <-- Product & Catalog functions ($PROD_)
│
└── /tests/                      <-- Automated Test Scripts ($s_)
    ├── TC01_Create_Account.sah
    ├── TC02_Submit_Order.sah
    └── TC03_Update_Inventory.sah
```