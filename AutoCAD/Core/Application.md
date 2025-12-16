# Application Class

## Overview
The `Application` class is the root object for accessing the AutoCAD application and its services. It provides access to documents, UI elements, preferences, and system-level functionality.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Inheritance Hierarchy
```
System.Object
  └─ Application (static class)
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `DocumentManager` | `DocumentManager` | Gets the document manager |
| `MainWindow` | `MainWindow` | Gets the main AutoCAD window |
| `MenuBar` | `MenuBar` | Gets the menu bar |
| `MenuGroups` | `MenuGroups` | Gets the menu groups collection |
| `Preferences` | `PreferencesFiles` | Gets application preferences |
| `Publisher` | `Publisher` | Gets the publisher object |
| `StatusBar` | `StatusBar` | Gets the status bar |
| `UserConfigurationManager` | `UserConfigurationManager` | Gets user configuration |
| `InfoCenter` | `InfoCenter` | Gets the InfoCenter |
| `Version` | `string` | Gets the AutoCAD version |
| `AcadApplication` | `object` | Gets the COM application object |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `ShowAlertDialog(string)` | `void` | Shows an alert dialog |
| `ShowModalDialog(Form)` | `DialogResult` | Shows a modal dialog |
| `ShowModelessDialog(Form)` | `void` | Shows a modeless dialog |
| `Idle` | `event` | Fires when application is idle |

## Code Examples

### Example 1: Accessing the Application
```csharp
using Autodesk.AutoCAD.ApplicationServices;

// Get the document manager
DocumentManager docMgr = Application.DocumentManager;

// Get the active document
Document acDoc = docMgr.MdiActiveDocument;

// Get AutoCAD version
string version = Application.Version;
ed.WriteMessage($"\nAutoCAD Version: {version}");
```

### Example 2: Accessing Main Window
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;

// Get main window for dialog parent
MainWindow mainWin = Application.MainWindow;

// Use as parent for WPF dialogs
System.Windows.Window myDialog = new System.Windows.Window();
myDialog.Owner = mainWin;
myDialog.ShowDialog();
```

### Example 3: Working with Status Bar
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

// Set status bar text
statusBar.SetMessageString("Processing...");

// Show progress
for (int i = 0; i <= 100; i += 10)
{
    statusBar.SetProgressMeter($"Progress: {i}%");
    System.Threading.Thread.Sleep(100);
}

statusBar.SetMessageString("Complete!");
```

### Example 4: Accessing Preferences
```csharp
using Autodesk.AutoCAD.ApplicationServices;

PreferencesFiles prefs = Application.Preferences.Files;

// Get support paths
string supportPaths = prefs.SupportPath;
ed.WriteMessage($"\nSupport paths: {supportPaths}");

// Get template path
string templatePath = prefs.QNewTemplateFile;
ed.WriteMessage($"\nDefault template: {templatePath}");
```

### Example 5: Using Idle Event
```csharp
using Autodesk.AutoCAD.ApplicationServices;

public void RegisterIdleEvent()
{
    Application.Idle += OnIdle;
}

private void OnIdle(object sender, EventArgs e)
{
    // Code to run when AutoCAD is idle
    // Be careful - this fires frequently!
}
```

### Example 6: Showing Alert Dialog
```csharp
using Autodesk.AutoCAD.ApplicationServices;

Application.ShowAlertDialog("This is an alert message!");
```

### Example 7: Accessing Menu Bar
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;

MenuBar menuBar = Application.MenuBar;

// Access menu groups
MenuGroups menuGroups = Application.MenuGroups;

foreach (MenuGroup mg in menuGroups)
{
    ed.WriteMessage($"\nMenu Group: {mg.Name}");
}
```

## Application Subobjects

### MainWindow
Represents the main AutoCAD application window. Used as parent for dialogs.

### MenuBar
Provides access to the AutoCAD menu bar for customization.

### MenuGroups
Collection of menu groups loaded in AutoCAD.

### Preferences
Access to all AutoCAD preferences and settings:
- Files preferences
- Display preferences
- Open and Save preferences
- System preferences
- User preferences

### Publisher
Handles batch plotting and publishing operations.

### StatusBar
Controls the status bar display and progress indicators.

### UserConfigurationManager
Manages user-specific configuration settings.

### InfoCenter
Access to the InfoCenter search and help system.

## Related Objects
- [DocumentManager](DocumentManager.md) - Manages open documents
- [Document](Document.md) - Individual drawing document
- [Database](Database.md) - Drawing database
- [Editor](Editor.md) - User interaction

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_Application)
