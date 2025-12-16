# SystemDialogs Class

## Overview
This documentation covers the standard system dialogs provided by the `Autodesk.AutoCAD.Windows` namespace, allowing developers to invoke native AutoCAD pickers for files, colors, and linetypes.

## Namespace
`Autodesk.AutoCAD.Windows`

## Key Classes

| Class | Description |
|-------|-------------|
| `OpenFileDialog` | Standard file open dialog. |
| `SaveFileDialog` | Standard file save dialog. |
| `ColorDialog` | AutoCAD Color Index (ACI) and TrueColor picker. |
| `LinetypeDialog` | Linetype selection dialog. |
| `LayerDialog` | Layer selection dialog (via internal UI or custom). |

## Code Examples

### Example 1: Open File Dialog
```csharp
using Autodesk.AutoCAD.Windows;

OpenFileDialog ofd = new OpenFileDialog(
    "Select Configuration", 
    null, 
    "xml;json", 
    "SettingsFiles", 
    OpenFileDialog.OpenFileDialogFlags.DoNotTransferRemoteFiles
);

System.Windows.Forms.DialogResult dr = ofd.ShowDialog();

if (dr == System.Windows.Forms.DialogResult.OK)
{
    string selectedFile = ofd.Filename;
    // Process file
}
```

### Example 2: Save File Dialog
```csharp
SaveFileDialog sfd = new SaveFileDialog(
    "Export Data",
    "report",
    "csv",
    "ExportFile",
    SaveFileDialog.SaveFileDialogFlags.None
);

if (sfd.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    string savePath = sfd.Filename;
}
```

### Example 3: Color Picker
```csharp
ColorDialog cd = new ColorDialog();
cd.Color = Autodesk.AutoCAD.Colors.Color.FromColorIndex(ColorMethod.ByAci, 1); // Default Red
cd.IncludeByBlockByLayer = true; // Allow ByLayer/ByBlock

if (cd.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    Autodesk.AutoCAD.Colors.Color selected = cd.Color;
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nSelected: {selected.ColorIndex}");
}
```

### Example 4: Linetype Picker
```csharp
LinetypeDialog ltd = new LinetypeDialog();
// ltd.LinetypeId = ...; // Set default

if (ltd.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    ObjectId linetypeId = ltd.LinetypeId;
    // Apply linetype
}
```

### Example 5: MessageBox (Standard)
```csharp
// AutoCAD provides standard MessageBox integration but usually you use standard Forms/WPF
// For AutoCAD-style alert:
Application.ShowAlertDialog("Critical Error Occurred!");
```

### Example 6: Layer State Dialog (Advanced)
```csharp
// There is no simplistic "LayerDialog" class exposed directly like ColorDialog.
// Usually developers build a custom form listing layers from the database,
// or use COM interop to invoke built-in dialogs.
```

### Example 7: Handling Dialog Results
```csharp
// Dialogs return System.Windows.Forms.DialogResult
// Remember to reference System.Windows.Forms
if (result == System.Windows.Forms.DialogResult.Cancel)
{
    // User cancelled
    return;
}
```

### Example 8: Folder Browser
```csharp
// For folder selection, use standard .NET 
// System.Windows.Forms.FolderBrowserDialog
// AutoCAD does not provide a specific override for this.
```

## Best Practices
1. **Modal Context**: These dialogs are modal. They block the AutoCAD UI until closed.
2. **References**: Requires `System.Windows.Forms` reference in your project.
3. **UI Thread**: Always show dialogs from the main UI thread.

## Related Objects
- [Editor](../Core/Editor.md)
- [Application](../Core/Application.md)

## References
- [Autodesk Windows Namespace](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows)
