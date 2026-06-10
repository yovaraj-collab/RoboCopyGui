# Yova RoboCopy GUI

**Version:** 1.1 EN  
**Author:** Yovaraj / Jörn Walter (GUI extension)  
**Created:** June 10, 2026  
**License:** © 2026 Yovaraj. All rights reserved. — [https://linktr.ee/yovaraj](https://linktr.ee/yovaraj)

---

## Overview

RoboCopy GUI is a Windows GUI application built in PowerShell that wraps the native `robocopy` utility with a user-friendly interface. It provides a single window for copying, synchronizing, comparing, and cleaning folders — including support for local paths and UNC network paths. No administrator elevation is required to run the tool.

<img width="982" height="717" alt="Screenshot 2026-06-10 125325" src="https://github.com/user-attachments/assets/ccbe53b9-2d00-4291-9329-a6fc76fbf03b" />

---

## Requirements

- Windows 10 or later
- PowerShell 5.1 or later
- .NET Framework (Windows Forms — included with Windows)
- `robocopy` (included with Windows)

---

## How to Run

### Option 1 — BAT Launcher (Recommended)

The easiest way to launch the tool is by double-clicking **`Launch_RoboCopy_GUI.bat`**. Make sure both `Launch_RoboCopy_GUI.bat` and `source.ps1` are in the same folder. The launcher starts PowerShell silently in the background (no console window) with execution policy bypass, so no system policy changes are needed. If `source.ps1` is missing or PowerShell exits with an error, the launcher will display an informative message.

### Option 2 — Direct PowerShell

Right-click `source.ps1` and select **Run with PowerShell**, or launch from a PowerShell terminal:

```powershell
.\source.ps1
```

If you encounter an execution policy error, run:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

---

## Features

### Folder Selection
Set the **Source** and **Target** folders using the text fields or the **Browse** buttons. Both local paths (e.g. `C:\Data`) and UNC network paths (e.g. `\\server\share`) are supported. The tool validates paths before any operation is started. The last 10 used source/target pairs are saved and available as dropdown suggestions for quick reuse.

### Create Folder Structure
Replicates the directory tree from source to target without copying file data. Useful for pre-creating a matching folder layout on a new drive or network share.

### Copy Data
Copies all files and subdirectories from source to target using `robocopy /E /COPY:DAT /MT:8`. Runs as a background job with a live progress bar. On completion, the path pair is saved to history.

### Synchronize
Mirrors the source to the target using `robocopy /MIR`, restricted to files modified within the last **x days** (configurable via the spinner, default 7). This keeps the target an up-to-date mirror of recent changes.

### Compare Folders
Runs `robocopy` in list-only mode (`/L`) to show differences between source and target without making any changes. Results are printed to the output log.

### Clean Target
Removes files and folders from the target that no longer exist in the source (`/PURGE`), without copying any new data. Requires confirmation before proceeding.

### Delete by Date
Deletes files from the target folder that match a specific date. The date picker lets you choose the target date, and radio buttons let you select whether to match by **creation date** or **last modified date**. Requires confirmation before proceeding.

### Delete by Pattern
Deletes all files in the target whose filename contains a specified text pattern (wildcard-style). Enter the pattern in the text field and click **Delete by Pattern in Target**. Requires confirmation before proceeding.

---

## Logging

A real-time output window at the bottom of the form shows timestamped messages for every operation. If **Create Robocopy log file** is checked (enabled by default), a `.log` file is also written to a `RCOPYLOGS` folder on your Desktop for each operation. Right-clicking the output window provides a **Copy to clipboard** option.

---

## Configuration

Path history is stored as JSON at:

```
%APPDATA%\RobocopyGUI\paths.json
```

Up to 10 recent source/target pairs are retained automatically.

---

## Notes

- Robocopy exit codes below 8 are treated as success; codes 8 and above trigger an error message.
- The progress bar during copy and sync operations shows an estimated value; exact progress is not available from robocopy in real time.
- All delete operations display a confirmation dialog before any files are removed.
