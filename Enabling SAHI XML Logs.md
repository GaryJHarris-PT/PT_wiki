  
If you see **DBReports** in the browser URL means your SAHI Pro instance is currently configured to log test results directly into SAHI's internal **database** rather than saving raw XML log files to disk.

---

How to Enable XML File Logs for `parse_logs.js`

To tell SAHI Pro to write XML log files to your hard drive, you just need to add `xml` to SAHI's configuration:

1. Open SAHI's configuration file in Notepad: `C:\sahi_pro\userdata\config\userdata.properties` _(or_ _C:\Users\<YourUsername>\sahi_pro\userdata\config\userdata.properties__)_
2. Look for the **reports.type** setting. If it's not there, add it to the bottom of the file:

```
reports.type=db,html,xml
```

1. Save the file and **restart SAHI Pro**.

---

What Happens Next

- You can keep using SAHI exactly as you do today—your browser reports (`DBReports`) will still work normally.
- Going forward, every time a script or suite finishes executing, SAHI will **also** save an XML log file into `userdata/logs/playback/`.
- When your test run completes, double-clicking `Run_Log_Parser.bat` will find those XML files and generate your timestamped `.csv` error summary automatically!