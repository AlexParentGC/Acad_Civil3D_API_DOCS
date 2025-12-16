# PaletteSet Class

## Overview
`PaletteSet` represents a floating or docked window in AutoCAD that can contain multiple tabs (Palettes). It is the standard mechanism for creating modeless, interactive UI tools like the Properties Palette, Layer Manager, or custom tool interfaces. It supports hosting standard .NET `UserControl`s (WinForms) or `ElementHost` (WPF).

## Namespace
`Autodesk.AutoCAD.Windows`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | The title of the palette set. |
| `Visible` | `bool` | Controls visibility. |
| `Style` | `PaletteSetStyles` | Controls docking, transparency, and snapping behavior. |
| `Dock` | `DockSides` | Gets or sets current docking position. |
| `Count` | `int` | Number of tabs (Palettes) in the set. |
| `Opacity` | `int` | Transparency level (0-100). |
| `KeepFocus` | `bool` | If true, palette keeps focus after interaction. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Add(string, Control)` | `Palette` | Adds a tab with a WinForms Control. |
| `AddVisual(string, Visual)` | `Palette` | Adds a WPF Visual (requires API adaptation). |
| `Close()` | `void` | Hides the palette (doesn't destroy it). |

## Code Examples

### Example 1: Creating a Basic PaletteSet
```csharp
using Autodesk.AutoCAD.Windows;
using System.Windows.Forms;

public class MyPlugin
{
    // Define as static to persist across commands
    private static PaletteSet _ps = null;

    [CommandMethod("ShowPalette")]
    public void ShowPalette()
    {
        if (_ps == null)
        {
            _ps = new PaletteSet("My Tools");
            _ps.Style = PaletteSetStyles.ShowAutoHideButton | 
                        PaletteSetStyles.ShowCloseButton |
                        PaletteSetStyles.Snappable;
            
            // Add a simple UserControl
            UserControl myControl = new UserControl();
            myControl.Controls.Add(new Button { Text = "Click Me", Dock = DockStyle.Top });
            
            _ps.Add("Tab 1", myControl);
        }
        
        _ps.Visible = true;
    }
}
```

### Example 2: Hosting WPF Content
```csharp
// Standard Host Control for WPF
public class WpfHostControl : System.Windows.Forms.UserControl
{
    private System.Windows.Forms.Integration.ElementHost _host;
    
    public WpfHostControl(System.Windows.Controls.UserControl wpfControl)
    {
        _host = new System.Windows.Forms.Integration.ElementHost();
        _host.Dock = DockStyle.Fill;
        _host.Child = wpfControl;
        this.Controls.Add(_host);
    }
}

// Usage in Command
_ps.Add("WPF Tab", new WpfHostControl(new MyWpfUserControl()));
```

### Example 3: Managing Docking
```csharp
// Force docking to the left
_ps.Dock = DockSides.Left;

// Restrict docking options (e.g., prevent floating)
_ps.DockEnabled = DockSides.Left | DockSides.Right;
```

### Example 4: Handling Events
```csharp
_ps.StateChanged += (s, e) => 
{
    if (e.NewState == StateEventIndex.Hide)
    {
         // Palette was hidden/collapsed
    }
};

_ps.PaletteActivated += (s, e) =>
{
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nSwitched to {e.Activated.Name}");
};
```

### Example 5: Saving State (Size/Position)
```csharp
// By default, AutoCAD saves PaletteSet state in the registry/profile based on the GUID.
// To ensure unique saving, create the PaletteSet with a specific GUID.
_ps = new PaletteSet("My Tools", new Guid("D3D8C3E0-5F9B-4B52-8E3A-1C2D3E4F5A6B"));
```

### Example 6: Removing Tabs
```csharp
if (_ps.Count > 0)
{
    // Remove tab at index 0
    _ps.Remove(0);
}
```

### Example 7: Modeless Interaction
```csharp
// Palettes are modeless. They can interact with the drawing while valid.
// IMPORTANT: You must Lock the document when modifying DB from a Palette event.
private void ButtonClick(object sender, EventArgs e)
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    
    // Lock document logic
    using (doc.LockDocument())
    {
        using (Transaction tr = doc.Database.TransactionManager.StartTransaction())
        {
            // Modify database...
            tr.Commit();
        }
    }
}
```

### Example 8: Transparency
```csharp
// Set 90% opacity
_ps.Opacity = 90;
```

## Best Practices
1. **Static Lifecycle**: Always keep a `static` reference to your `PaletteSet`. If you create a new one every command, you'll get duplicate windows.
2. **GUID**: Always provide a generic `Guid` in the constructor to ensure AutoCAD remembers the window position and size between sessions.
3. **Document Locking**: User interactions in the palette run in the UI thread context, not the command context. You **MUST** use `doc.LockDocument()` before starting a transaction.
4. **WPF**: Prefer WPF for modern UI. Use `ElementHost` to bridge the gap, as `PaletteSet` natively expects WinForms controls.

## Related Objects
- [Editor](../Core/Editor.md) - For feedback.
- [Application](../Core/Application.md) - For document access.

## References
- [Autodesk PaletteSet Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows_PaletteSet)
