# Preferences Class

## Overview
The `Preferences` class provides access to AutoCAD application preferences and settings, similar to the Options dialog. It contains multiple sub-objects for different categories of settings.

## Namespace
`Autodesk.AutoCAD.ApplicationServices` (accessed via COM)

## Inheritance Hierarchy
```
System.Object
  └─ AcadPreferences (COM object)
```

## Key Sub-Objects

| Sub-Object | Description |
|------------|-------------|
| `Files` | File paths and locations |
| `Display` | Display settings |
| `OpenSave` | Open and save settings |
| `Output` | Plotting and publishing settings |
| `System` | System settings |
| `User` | User preferences |
| `Drafting` | Drafting settings |
| `Selection` | Selection settings |
| `Profiles` | Profile management |

## Code Examples

### Example 1: Accessing File Preferences
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic filesPrefs = prefs.Files;

// Get support paths
string supportPath = filesPrefs.SupportPath;
ed.WriteMessage($"\nSupport Path: {supportPath}");

// Get template path
string templatePath = filesPrefs.QNewTemplateFile;
ed.WriteMessage($"\nDefault Template: {templatePath}");

// Get drawing file paths
string drawingPath = filesPrefs.DefaultInternetURL;
ed.WriteMessage($"\nDefault Drawing Path: {drawingPath}");
```

### Example 2: Modifying File Paths
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic filesPrefs = prefs.Files;

// Add to support path
string currentPath = filesPrefs.SupportPath;
string newPath = "C:\\MyCustomPath";

if (!currentPath.Contains(newPath))
{
    filesPrefs.SupportPath = currentPath + ";" + newPath;
    ed.WriteMessage($"\nAdded {newPath} to support path");
}
```

### Example 3: Display Preferences
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic displayPrefs = prefs.Display;

// Get display settings
bool showScrollBars = displayPrefs.DisplayScrollBars;
int crosshairSize = displayPrefs.CursorSize;

ed.WriteMessage($"\nShow Scrollbars: {showScrollBars}");
ed.WriteMessage($"\nCrosshair Size: {crosshairSize}%");

// Modify display settings
displayPrefs.CursorSize = 50; // Set to 50%
```

### Example 4: System Preferences
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic systemPrefs = prefs.System;

// Get system settings
bool singleDocMode = systemPrefs.SingleDocumentMode;
bool beepOnError = systemPrefs.BeepOnError;

ed.WriteMessage($"\nSingle Document Mode: {singleDocMode}");
ed.WriteMessage($"\nBeep on Error: {beepOnError}");
```

### Example 5: Open/Save Preferences
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic openSavePrefs = prefs.OpenSave;

// Get save settings
int saveInterval = openSavePrefs.AutoSaveInterval;
string autoSavePath = openSavePrefs.AutoSavePath;

ed.WriteMessage($"\nAuto-save Interval: {saveInterval} minutes");
ed.WriteMessage($"\nAuto-save Path: {autoSavePath}");

// Modify save settings
openSavePrefs.AutoSaveInterval = 10; // Set to 10 minutes
```

### Example 6: User Preferences
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic prefs = Application.Preferences;
dynamic userPrefs = prefs.User;

// Get user settings
int undoLevels = userPrefs.UndoLevels;
bool rightClickCustomization = userPrefs.SCMDefaultMode;

ed.WriteMessage($"\nUndo Levels: {undoLevels}");

// Modify user settings
userPrefs.UndoLevels = 100; // Set undo levels
```

## Common File Preferences Properties

| Property | Description |
|----------|-------------|
| `SupportPath` | Support file search path |
| `QNewTemplateFile` | Default template for QNEW |
| `DefaultInternetURL` | Default drawing location |
| `AutoSavePath` | Auto-save file location |
| `TempFilePath` | Temporary files location |
| `LogFilePath` | Log file location |
| `PlotLogFilePath` | Plot log file location |

## Common Display Preferences Properties

| Property | Description |
|----------|-------------|
| `DisplayScrollBars` | Show scroll bars |
| `CursorSize` | Crosshair cursor size (1-100) |
| `LayoutDisplayMargins` | Show margins in layouts |
| `LayoutShowPlotSetup` | Show plot setup in layouts |

## Common System Preferences Properties

| Property | Description |
|----------|-------------|
| `SingleDocumentMode` | SDI mode enabled |
| `BeepOnError` | Beep on error |
| `ShowWarningMessages` | Show warning messages |

## Related Objects
- [Application](Application.md) - Provides access to Preferences

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-AcadPreferences)
