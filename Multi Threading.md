# Me
for scripts that depend on previous scripts having run successfully, how is that handled when the test suite will be run with multiple concurrent threads?

# Response
When scaling up SAHI Pro test suites to run with **multiple concurrent threads**1, managing dependent scripts requires controlling execution order and isolating test data so parallel browser threads don't execute steps out of order or collide on database records.

Here are the standard ways this is handled in SAHI Pro:

---

1. Bundle Dependent Steps into Sub-Suites (Sequential Thread Execution)

In SAHI Pro's multithreaded suite runner, threads pull work items from the suite execution list.

- Instead of putting loose dependent `.sah` scripts into the main parallel queue, group dependent steps sequentially into dedicated sub-suite files (e.g., `kitting_wha.suite`)2.
- When Thread 1 picks up `kitting_wha.suite`, it executes `01_Receive_WHA.sah` $\rightarrow$ `02_Create_Kit_WHA.sah` $\rightarrow$ `03_Issue_Kit_WHA.sah` in exact sequential order2. Meanwhile, Thread 2 picks up `kitting_whb.suite` and executes its sequence concurrently.

---

2. Master Driver Scripts per Thread

Another clean pattern is to write a single wrapper script to serve as the entry point for a thread:

```
// run_kitting_wha.sah
_include("01_Receive_Stock_WHA.sah");
_include("02_Create_Kit_WHA.sah");
_include("03_Issue_Kit_WHA.sah");
```

In your multithreaded suite configuration, you list only the master driver scripts (`run_kitting_wha.sah`, `run_kitting_whb.sah`). Each thread executes its master wrapper sequentially, maintaining the dependency chain within that thread while running in parallel with other browser threads.

---

3. Ensure Data & Warehouse Isolation Across Threads

When multiple threads execute in RepairData simultaneously:

- **Avoid shared data conflicts:** If Thread 1 and Thread 2 attempt to modify or receive stock for the exact same Item Number or Bin at the exact same time, database locks or validation errors can occur.
- **Scope by domain/warehouse:** Ensure each thread operates on independent data sets (e.g., Thread 1 targets Warehouse A, Thread 2 targets Warehouse B, or scripts generate unique dynamic serial/item numbers).

---

4. Intra-Thread Data Handoff (`_setGlobal` or File Persistence)

When Script 2 depends on dynamic values created by Script 1 (such as a Work Order Number or LID generated on screen)3:

- **_setGlobal()** **/** **_getGlobal()****:** Script 1 stores the created ID in memory via `_setGlobal("g_WHA_WO", $woNum)`, and Script 2 retrieves it via `_getGlobal("g_WHA_WO")` when it starts up on the same thread3.
- **File Handoff:** Script 1 writes dynamic IDs to a temporary text/CSV file via `_writeFile()`, which Script 2 reads using `_readFile()` at step startup3.