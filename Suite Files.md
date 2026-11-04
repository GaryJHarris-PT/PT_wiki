# Me
currently the 'suites' I run are just .sah files with a list of path/filename entries for Sahi to run in order. .suite files are different? 

# Response

**Functionally,** **.suite** **files are SAHI Pro's native test suite container files that unlock SAHI's Suite Runner engine.**

When you put a list of script paths inside a `.sah` file (or run steps consecutively using `_include()`), SAHI processes them sequentially within a single continuous browser execution.

Switching to formal **.suite** files provides three key architectural advantages:

- **Independent Session Lifecycle**: When SAHI Pro executes a `.suite` file, it treats each `.sah` script entry as an independent test run with a fresh browser session, clean DOM, and isolated ViewState.
- **Multithreading & Parallel Execution**: SAHI Pro's Suite Runner uses `.suite` files to distribute scripts across multiple parallel threads and separate execution machines.
- **Suite Reporting & Automated Retries**: Running a `.suite` file enables SAHI's built-in suite reporter, which logs results per script and generates retry suite files (such as `failed.suite`) for re-running failed steps.

Recommended `.suite` File Format

A `.suite` file is a plain text file containing script file paths (one per line):

```
tests/Kitting_WHA/01_Receive_Stock_WHA.sah
tests/Kitting_WHA/02_Create_Kit_WHA.sah
tests/Kitting_WHA/03_Issue_Kit_WHA.sah
```