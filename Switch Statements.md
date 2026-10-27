In Sahi Pro, scripts (`.sah` files) use **standard JavaScript syntax**, so you write a `switch` statement exactly as you would in JavaScript.

Here is the general syntax and a practical example tailored to SAHI automation scripts:

---

Basic `switch` Syntax in SAHI Pro

```
switch ($variable) {
    case "Value1":
        // Actions for Value1
        break;
        
    case "Value2":
        // Actions for Value2
        break;
        
    default:
        // Default actions if no case matches
        break;
}
```

---

Practical Example: Branching Actions by Warehouse Code

Here is how you might use a `switch` statement in a library helper function or script (for example, taking different setup steps depending on the selected Warehouse):

```
var $s_Warehouse = "STGMAC";

switch ($s_Warehouse) {
    case "STGMAC":
        _log("Configuring STGMAC warehouse parameters...");
        _setSelected(_select("ctl00$mc$ddlWarehouse"), "STGMAC, STG MATERIALS");
        break;

    case "WHA":
        _log("Configuring Warehouse A parameters...");
        _setSelected(_select("ctl00$mc$ddlWarehouse"), "WHA-01, MAIN WAREHOUSE");
        break;

    case "WHB":
        _log("Configuring Warehouse B parameters...");
        _setSelected(_select("ctl00$mc$ddlWarehouse"), "WHB-02, SECONDARY WAREHOUSE");
        break;

    default:
        _log("Warning: Unknown warehouse code '" + $s_Warehouse + "'. Defaulting to primary.");
        _setSelected(_select("ctl00$mc$ddlWarehouse"), "STGMAC, STG MATERIALS");
        break;
}
```

---

Key Tips to Remember:

1. **Don't Forget** **break;****:** If you omit `break;`, SAHI will "fall through" and execute the code in subsequent `case` blocks even if they don't match.
2. **Strict Equality:** JavaScript `switch` uses strict equality (`===`) to compare values, so string casing must match exactly (e.g., `"STGMAC"` vs `"stgmac"`).
3. **Always Include** **default:****:** Adding a `default:` block ensures your script logs a warning or handles unexpected values gracefully without failing silently.