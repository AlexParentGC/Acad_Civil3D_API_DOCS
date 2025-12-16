# Ribbon Interface

## Overview
The Ribbon API allows developers to programmatically customize the AutoCAD Ribbon interface (Tabs, Panels, and Buttons). This functionality is provided by the `AdWindows.dll` assembly (namespace `Autodesk.Windows`), which is separate from the standard AutoCAD API DLLs.

> [!IMPORTANT]
> **AdWindows.dll Required**: To use these features, you must add a reference to `AdWindows.dll` found in the AutoCAD installation directory.

## Namespace
`Autodesk.Windows`

## Key Classes

| Class | Description |
|-------|-------------|
| `ComponentManager` | Static manager to access the `RibbonControl`. |
| `RibbonControl` | Represents the entire Ribbon UI. |
| `RibbonTab` | A specific tab (e.g., "Home", "Plug-ins"). |
| `RibbonPanel` | A container within a tab. |
| `RibbonButton` | A clickable button element. |
| `RibbonPanelSource` | Defined content for a panel. |

## Code Examples

### Example 1: Creating a Custom Tab and Panel
```csharp
using Autodesk.Windows;
using System.Windows.Media.Imaging; // For icons

public void CreateRibbon()
{
    RibbonControl ribbon = ComponentManager.Ribbon;
    if (ribbon == null) return;

    // 1. Create Tab
    RibbonTab tab = new RibbonTab();
    tab.Title = "My Tools";
    tab.Id = "MY_TOOLS_TAB";
    
    // 2. Create Panel Source (Content)
    RibbonPanelSource panelSource = new RibbonPanelSource();
    panelSource.Title = "General";
    
    // 3. Create Panel (Visual container)
    RibbonPanel panel = new RibbonPanel();
    panel.Source = panelSource;
    
    // 4. Add items to Panel Source
    RibbonButton btn = new RibbonButton();
    btn.Text = "Run Cmd";
    btn.ShowText = true;
    btn.CommandParameter = "MYCOMMAND "; // Note space
    btn.CommandHandler = new AdskCommandHandler(); // Optional custom handler
    
    panelSource.Items.Add(btn);
    
    // 5. Assemble
    tab.Panels.Add(panel);
    ribbon.Tabs.Add(tab);
    
    // 6. Set Active
    tab.IsActive = true;
}
```

### Example 2: Adding a Button to an Existing Tab
```csharp
public void AddToAddinsTab()
{
    RibbonControl ribbon = ComponentManager.Ribbon;
    RibbonTab addinsTab = null;

    // Find "Add-ins" tab
    foreach (RibbonTab tab in ribbon.Tabs)
    {
        if (tab.Id == "ACAD.ADDINS") // Standard ID
        {
            addinsTab = tab;
            break;
        }
    }

    if (addinsTab != null)
    {
        // Add custom panel to existing tab
        // ... (create panel logic) ...
        addinsTab.Panels.Add(myPanel);
    }
}
```

### Example 3: Defining Command Handler
```csharp
// Standard practice: Buttons send command strings to AutoCAD
// But you can also intercept clicks directly using ICommand
public class AdskCommandHandler : System.Windows.Input.ICommand
{
    public bool CanExecute(object parameter) => true;
    public event EventHandler CanExecuteChanged;

    public void Execute(object parameter)
    {
        if (parameter is RibbonButton btn)
        {
            // Custom C# logic here
            System.Windows.MessageBox.Show("Clicked " + btn.Text);
        }
    }
}
```

### Example 4: Adding Icons
```csharp
// Ribbons use WPF ImageSources
btn.Image = LoadImage("my_icon_16.png");
btn.LargeImage = LoadImage("my_icon_32.png");
btn.Size = RibbonItemSize.Large;

// Helper to load resource
private System.Windows.Media.ImageSource LoadImage(string resourceName)
{
    // ... WPF BitmapImage loading logic ...
    return new BitmapImage(new Uri("pack://application:,,,/MyAssembly;component/" + resourceName));
}
```

### Example 5: Creating Split Buttons
```csharp
RibbonSplitButton splitBtn = new RibbonSplitButton();
splitBtn.Text = "Options";
splitBtn.ShowText = true;

splitBtn.Items.Add(new RibbonButton { Text = "Option A" });
splitBtn.Items.Add(new RibbonButton { Text = "Option B" });
```

### Example 6: Managing Ribbon State
```csharp
// Check if Ribbon exists (it might be closed by user)
if (ComponentManager.Ribbon == null) return;

// Force rebuild/update layout if dynamic changes don't show
// usually not needed if added to collections correctly.
```

### Example 7: Tooltips
```csharp
btn.ToolTip = "This is a basic tooltip";
// Enhanced content
RibbonToolTip extendedTip = new RibbonToolTip();
extendedTip.Title = "Detailed Help";
extendedTip.Content = "This command performs complex calculation.";
extendedTip.ExpandedContent = "More details...";
// btn.ToolTip = extendedTip; // Assign object
```

### Example 8: Creating Row Panels (Flow)
```csharp
RibbonRowPanel row = new RibbonRowPanel(); 
// Adds items horizontally in the panel
row.Items.Add(btn1);
row.Items.Add(btnSeparator);
row.Items.Add(btn2);
panelSource.Items.Add(row);
```

## Best Practices
1. **CUIx vs API**: It is generally easier and more robust to define Ribbons using a `.cuix` (Custom User Interface) file rather than hard-coding C# generation. The API is best for *dynamic* modifications at runtime.
2. **References**: You must reference `AdWindows.dll`, `PresentationCore`, `PresentationFramework`, and `WindowsBase`.
3. **Identifiers**: Use unique IDs for tabs and panels to avoid conflicts with other plugins.
4. **Icons**: Use embedded resources for icons to keep deployment simple.

## Related Objects
- [PaletteSet](PaletteSet.md)
- [Application](../Core/Application.md)

## References
- [AdWindows API (Unofficial Docs exist, official is sparse)](https://help.autodesk.com/view/OARX/2024/ENU/)
