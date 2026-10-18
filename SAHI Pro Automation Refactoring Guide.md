

_Pillar 3: Dynamic Element Identifier Neutralization in Production Suites_

# 1. Executive Summary & Safety Assessment

When maintaining active, live production test suites in SAHI Pro, automated test scripts frequently fail due to modified ASP.NET element identifiers or dynamic DOM rendering. Applying **Pillar 3: Dynamic Element Identifier Neutralization** directly within your live resolution workflow is not only completely safe—it is the recommended strategy to prevent recurring script breaks across application updates.

|   |
|---|
|**Key Takeaway:** Centralizing dynamic element neutralization inside shared module libraries (e.g., inventory_lib.sah or global_lib.sah) fixes entire clusters of failing production scripts simultaneously while permanently isolating individual scripts from future UI markup changes.|

# 2. Understanding Dynamic Element Identifier Neutralization

Standard SAHI Pro script recording often hardcodes explicit element accessors like **_span("ctl00_mc_lblMA")** or **_textbox("ctl00_mc_txtSearchItem")**. In ASP.NET applications, these prefixes (e.g., ctl00_mc_) can shift whenever page layouts, master pages, or control containers are altered during software updates.

## The Risk of Hardcoded Script Patching

Updating dynamic identifiers directly inside individual test scripts creates high technical debt. If 15 separate test scripts interact with the same inventory form, a single dynamic ID change requires 15 individual file edits. Sticking strictly to script-level patching leaves live production scripts vulnerable to repetitive breaks in subsequent test runs.

## The Neutralization Strategy

Dynamic Element Identifier Neutralization abstracts fragile DOM selectors into resilient, shared element anchors within module libraries. By using relative accessors (such as **_in()**, **_near()**) or regular expression pattern matching, SAHI scripts can locate target elements reliably regardless of dynamic container prefixes.

# 3. Step-by-Step Production Resolution Workflow

To safely neutralize dynamic elements during your live SAHI resolution cycle without disturbing running test suites, follow this four-step process:

|   |   |   |
|---|---|---|
|**Step**|**Phase Name**|**Action & Technical Methodology**|
|**Step 1**|**Pattern Tracking & Failure Grouping**|Collect error logs from the full SAHI suite execution. Group scripts sharing identical failure signatures (e.g., missing header span or broken button lookup) into a single pattern bucket.|
|**Step 2**|**Centralize & Neutralize in Library**|Define a shared element anchor variable inside inventory_lib.sah or global_lib.sah using relative accessors or regex. Replace hardcoded IDs across library functions.|
|**Step 3**|**Prove the Base Case (n = 1)**|Comment out legacy include/accessor lines in one failing script. Run that single script interactively using the SAHI Controller to verify element resolution on the live UI.|
|**Step 4**|**Batch Suite Re-run**|Mark all affected pattern scripts as 'AGAIN', compile them into an ad hoc suite, and dispatch the suite to Machine 2 for background execution.|

# 4. Code Implementation & Anchor Patterns

## Legacy Fragile Selector vs. Neutralized Anchor

Consider a scenario where an inventory header element fails due to an ASP.NET dynamic ID change:

**// ❌ Fragile Hardcoded Script Selector:  
**var $INV_HEADER_LABEL_SPAN = _span("ctl00_mc_lblMA");

**// ✅ Neutralized & Resilient Library Anchor:  
**var $INV_HEADER_LABEL_SPAN = _span(/.*lblMA$/);  
// Or using relative SAHI positional accessors:  
var $INV_HEADER_LABEL_SPAN = _span(0, _in(_div("inventoryHeader")));

# 5. Integration with Parallel Testing Architecture

By combining Dynamic Element Neutralization with your dual-machine SAHI execution workflow, you achieve maximum refactoring speed and zero testing downtime:

**• Machine 1 (Foreground / Controller):** Used to edit library definitions, comment out obsolete script includes, and interactively verify the base case (n = 1) using the SAHI Controller.

**• Machine 2 (Background / Runner):** Dedicated to executing verified batch suites continuously in the background, keeping your primary workspace responsive.