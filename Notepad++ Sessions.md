Notepad++ session files store open tab paths, caret positions, bookmarks, and dual-view states formatted as XML under the hood.

# Key themes:

- **Do Not Use .sah for Session Files**: .sah is the dedicated script extension for Sahi Pro test automation code. Giving a session file a .sah extension will cause Sahi Pro's runner/controller to fail if it attempts to execute the XML session file as JavaScript test code.

- **Recommended Session Extensions**: Standard recommended extensions for Notepad++ session files are **.session**, **.xml**, or **.nps** (Notepad Session).

- **Automating Session Loading in Notepad++**: You can register your extension by going to **Settings > Preferences > MISC.** in Notepad++ and typing session (or your chosen extension) into the **Session file ext.** box. Once set, double-clicking or loading files with that extension will automatically trigger Notepad++ to open the group of tabs rather than displaying the raw XML code.
