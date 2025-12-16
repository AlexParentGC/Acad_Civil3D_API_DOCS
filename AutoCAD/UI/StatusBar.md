# StatusBar Class

## Overview
The `StatusBar` functions allow plugins to display information in the main AutoCAD application window's status bar (bottom right). You can add custom panes (icons/text) or control the progress meter to show operation status.

## Namespace
`Autodesk.AutoCAD.Windows` / `Autodesk.AutoCAD.Runtime`

## Key Classes
- `Application.StatusBar` (Access point)
- `StatusBarItem` (Legacy/Pane control)
- `ProgressMeter` (Long operation feedback)

## Code Examples

### Example 1: Using the Progress Meter
```csharp
public void ProcessHeavyData()
{
    ProgressMeter pm = new ProgressMeter();
    pm.Start("Processing entities...");
    pm.SetLimit(100);

    for (int i = 0; i < 100; i++)
    {
        System.Threading.Thread.Sleep(50); // Simulate work
        pm.MeterProgress();
        System.Windows.Forms.Application.DoEvents(); // Keep UI responsive (Use carefully)
    }

    pm.Stop();
}
```

### Example 2: Adding a Status Bar Pane
```csharp
// NOTE: Status Bar customization API varies significantly by version.
// Modern AutoCAD (2015+) restricts adding arbitrary panes compared to older versions.
// The raw API usually involves the StatusBarItem class.

/*
StatusBarItem item = new StatusBarItem();
item.Text = "MyPlugin Active";
item.ToolTipText = "Plugin Status";
item.Icon = ...;
Application.StatusBar.Panes.Add(item);
*/
```

### Example 3: Updating Pane Text
```csharp
/*
item.Text = "Updated Status";
item.Update(); // Refresh UI
*/
```

### Example 4: Handling Pane Clicks
```csharp
/*
item.MouseDown += (s, e) => 
{
    Application.ShowAlertDialog("You clicked the status pane!");
};
*/
```

### Example 5: Basic Status Text
```csharp
// Simplest way to show status is writing to Command Line, not Status Bar
Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nDone.");

// But you can also set the 'MODEMACRO' system variable which displays text in the bottom left
Application.SetSystemVariable("MODEMACRO", "MyPlugin Configured");
```

### Example 6: Progress Meter Cancellation
```csharp
// Check for user cancellation (Escape key) manually during loops
if (HostApplicationServices.Current.UserBreak())
{
    pm.Stop();
    throw new Exception("Cancelled by user");
}
```

### Example 7: Nested Progress
```csharp
// AutoCAD only supports ONE active progress meter at a time.
// Do not try to nest them.
```

### Example 8: Cleaning Up
```csharp
// Always call Stop() in a finally block to ensure the meter disappears
// even if code crashes.
try
{
    pm.Start("Working...");
    // work
}
finally
{
    pm.Stop(); // Hides the bar
}
```

## Best Practices
1. **Use `try/finally` for Progress**: Failing to stop the progress meter can leave it stuck on the screen until restart.
2. **MODEMACRO**: For simple text feedback, setting the `MODEMACRO` variable is far easier and more reliable than creating custom StatusBar panes.
3. **Responsiveness**: The progress meter only updates if the UI thread pumps messages. In tight loops, you might need `DoEvents` (use sparingly) or run logic on a background thread (advanced).

## Related Objects
- [Application](../Core/Application.md)

## References
- [Autodesk Runtime Namespace](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Runtime)
