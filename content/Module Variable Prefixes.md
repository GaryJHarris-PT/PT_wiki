**$WO_** **is a fantastic choice**, and using 2–3 letter module prefixes is a standard best practice in modular test frameworks.

Instead of keeping everything under generic `$CFG_` or making up inconsistent names, prefixing shared module constants and default data by domain keeps your library files clean, self-documenting, and collision-free.

---

Standard Module Prefix Examples

|Domain / Library File|Prefix|Example Variables|
|---|---|---|
|**Global / Framework Config**|`$CFG_`|`$CFG_BaseUrl`, `$CFG_Browser`|
|**Work Orders** (`work_orders_lib.sah`)|`$WO_`|`$WO_DefaultPriority`, `$WO_Status_Open`, `$WO_Level_Depot`|
|**Inventory / Items** (`inventory_lib.sah`)|`$INV_`|`$INV_DefaultWarehouse`, `$INV_UnitOfMeasure`|
|**Assets / Configurations** (`assets_lib.sah`)|`$AST_`|`$AST_DefaultTemplate`, `$AST_BranchCode_00`|

---

The Complete Variable Scope Reference

With `$WO_` added, your full naming system looks like this:

```
// 1. Module Shared Constants (in work_orders_lib.sah)
var $WO_DefaultFailureCode = "CM, COMM T SYSTEM";

// 2. Script-Level Variable (in main test script)
var $s_WorkOrderNumber = "WO-2026-001";

// 3. Helper Function Signature & Body
function createWorkOrder($p_FailureCode, $p_WONumber) {
    var $localStatus = _getText(_textbox("ctl00$mc$ddlStatus"));
    
    if ($localStatus == "Please select") {
        _setValue(_textbox("ctl00$mc$txtFailureCode"), $p_FailureCode);
    }
}
```

This gives you a rock-solid, professional convention across your entire test suite.