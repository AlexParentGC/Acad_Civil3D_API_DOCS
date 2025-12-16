# MainWindow Class

## Overview
The `MainWindow` class represents the main AutoCAD application window. It's primarily used as a parent window for dialogs to ensure they appear correctly within the AutoCAD interface.

## Namespace
`Autodesk.AutoCAD.Windows`

## Inheritance Hierarchy
```
System.Object
  └─ System.Windows.Window
      └─ MainWindow
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Title` | `string` | Gets/sets the window title |
| `Width` | `double` | Gets/sets the window width |
| `Height` | `double` | Gets/sets the window height |
| `Left` | `double` | Gets/sets the left position |
| `Top` | `double` | Gets/sets the top position |

## Code Examples

### Example 1: Using MainWindow as Dialog Parent
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;
using System.Windows;

// Get main window
MainWindow mainWin = Application.MainWindow;

// Create WPF dialog
Window myDialog = new Window();
myDialog.Title = "My Dialog";
myDialog.Width = 400;
myDialog.Height = 300;
myDialog.Owner = mainWin; // Set AutoCAD as parent
myDialog.WindowStartupLocation = WindowStartupLocation.CenterOwner;

// Show dialog
myDialog.ShowDialog();
```

### Example 2: Using with Windows Forms
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;
using System.Windows.Forms;
using System.Windows.Interop;

MainWindow mainWin = Application.MainWindow;

// Create Windows Forms dialog
Form myForm = new Form();
myForm.Text = "My Form";
myForm.Width = 400;
myForm.Height = 300;

// Set AutoCAD as parent using handle
WindowInteropHelper helper = new WindowInteropHelper(mainWin);
IWin32Window owner = new Win32Window(helper.Handle);
myForm.ShowDialog(owner);
```

### Example 3: Getting Window Information
```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Windows;

MainWindow mainWin = Application.MainWindow;

ed.WriteMessage($"\nAutoCAD Window Title: {mainWin.Title}");
ed.WriteMessage($"\nWindow Size: {mainWin.Width} x {mainWin.Height}");
ed.WriteMessage($"\nWindow Position: ({mainWin.Left}, {mainWin.Top})");
```

## Helper Class for Windows Forms

```csharp
public class Win32Window : IWin32Window
{
    private IntPtr _handle;
    
    public Win32Window(IntPtr handle)
    {
        _handle = handle;
    }
    
    public IntPtr Handle
    {
        get { return _handle; }
    }
}
```

## Related Objects
- [Application](Application.md) - Provides access to MainWindow
- [Document](Document.md) - Document window

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows_MainWindow)
