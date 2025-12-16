# StatusBar Class

## Overview
The `StatusBar` class provides access to AutoCAD's status bar for displaying messages and progress indicators.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Inheritance Hierarchy
```
System.Object
  └─ StatusBar
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `SetMessageString(string)` | `void` | Sets the status bar message |
| `SetProgressMeter(string)` | `void` | Sets progress meter text |
| `SetProgressMeterLimit(int)` | `void` | Sets progress meter maximum value |
| `SetProgressMeterPos(int)` | `void` | Sets progress meter position |
| `ShowProgressMeter()` | `void` | Shows the progress meter |
| `HideProgressMeter()` | `void` | Hides the progress meter |

## Code Examples

### Example 1: Simple Status Message
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

statusBar.SetMessageString("Processing data...");

// Do work
System.Threading.Thread.Sleep(2000);

statusBar.SetMessageString("Complete!");
```

### Example 2: Progress Meter
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

// Set up progress meter
statusBar.SetProgressMeterLimit(100);
statusBar.ShowProgressMeter();

for (int i = 0; i <= 100; i++)
{
    statusBar.SetProgressMeterPos(i);
    statusBar.SetMessageString($"Processing: {i}%");
    
    // Do work
    System.Threading.Thread.Sleep(50);
}

statusBar.HideProgressMeter();
statusBar.SetMessageString("Processing complete!");
```

### Example 3: Progress with Custom Range
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

int totalItems = 250;
statusBar.SetProgressMeterLimit(totalItems);
statusBar.ShowProgressMeter();

for (int i = 0; i < totalItems; i++)
{
    statusBar.SetProgressMeterPos(i);
    statusBar.SetMessageString($"Processing item {i + 1} of {totalItems}");
    
    // Process item
}

statusBar.HideProgressMeter();
statusBar.SetMessageString($"Processed {totalItems} items");
```

### Example 4: Progress with Try-Finally
```csharp
using Autodesk.AutoCAD.ApplicationServices;

StatusBar statusBar = Application.StatusBar;

try
{
    statusBar.SetProgressMeterLimit(100);
    statusBar.ShowProgressMeter();
    
    for (int i = 0; i <= 100; i++)
    {
        statusBar.SetProgressMeterPos(i);
        
        // Do work that might throw exception
        
        if (i == 50)
        {
            // Simulate error
            throw new System.Exception("Error at 50%");
        }
    }
}
catch (System.Exception ex)
{
    statusBar.SetMessageString($"Error: {ex.Message}");
}
finally
{
    // Always hide progress meter
    statusBar.HideProgressMeter();
}
```

## Best Practices

1. **Always Hide Progress Meter**: Use try-finally to ensure progress meter is hidden even if an error occurs
2. **Update Frequency**: Don't update too frequently (causes flickering), update every 1-5% is usually sufficient
3. **Clear Messages**: Set a final message when operation completes
4. **User Feedback**: Provide meaningful messages that describe what's happening

## Related Objects
- [Application](Application.md) - Provides access to StatusBar

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_ApplicationServices_StatusBar)
