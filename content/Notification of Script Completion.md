You can configure SAHI Pro to play a sound upon script completion or stoppage by defining SAHI's built-in `onScriptEnd()` and `onScriptFailure()` event callbacks in your `global_lib.sah` file.

Since SAHI Pro runs on a Java engine, you can invoke Java's native `java.awt.Toolkit.getDefaultToolkit().beep()` directly inside these callbacks without needing external audio files or OS-specific player dependencies.

---

### Recommended Implementation

Add these callback functions to your `global_lib.sah` file:

```
function onScriptEnd() {
    java.awt.Toolkit.getDefaultToolkit().beep();
}

function onScriptFailure($e) {
    java.awt.Toolkit.getDefaultToolkit().beep();
}
```

---

### How It Works

- **`onScriptEnd()`**: SAHI automatically executes this function whenever a script finishes playback or is manually stopped by the user.
- **`onScriptFailure($e)`**: SAHI executes this function if the script aborts due to an unhandled error or assertion failure.
- **`java.awt.Toolkit.getDefaultToolkit().beep()`**: Triggers the system's standard alert sound across Windows, Linux, and macOS directly from the SAHI execution environment.

What's next?