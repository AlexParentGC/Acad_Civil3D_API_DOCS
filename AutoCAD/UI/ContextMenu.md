# ContextMenu Class

## Overview
AutoCAD allows developers to append custom menu items to the context (right-click) menu of specific entity types or the default context menu. This is done via the `ContextMenuExtension` class.

## Namespace
`Autodesk.AutoCAD.Windows`

## Key Classes

| Class | Description |
|-------|-------------|
| `ContextMenuExtension` | A custom menu definition. |
| `MenuItem` | An individual clickable item. |
| `Application.AddObjectContextMenuExtension` | Registers menu for an entity type. |
| `Application.AddDefaultContextMenuExtension` | Registers menu for "nothing selected" state. |

## Code Examples

### Example 1: Adding Menu to Specific Entity Type
```csharp
using Autodesk.AutoCAD.Windows;
using Autodesk.AutoCAD.Runtime;

public class MyPlugin : IExtensionApplication
{
    private ContextMenuExtension _circleMenu;

    public void Initialize()
    {
        _circleMenu = new ContextMenuExtension();
        _circleMenu.Title = "Circle Tools"; // Submenu title (optional)
        
        MenuItem item = new MenuItem("Calculate Area Info");
        item.Click += OnCalculateArea;
        _circleMenu.MenuItems.Add(item);
        
        // Register for ALL Circle objects
        Application.AddObjectContextMenuExtension(
            RXObject.GetClass(typeof(Circle)), 
            _circleMenu
        );
    }

    public void Terminate()
    {
        // Always cleanup
        Application.RemoveObjectContextMenuExtension(
             RXObject.GetClass(typeof(Circle)), 
             _circleMenu
        );
    }

    private void OnCalculateArea(object sender, EventArgs e)
    {
        // Context logic
        // The selected objects are in the SelectionSet
        // ...
    }
}
```

### Example 2: Adding to Global Context Menu
```csharp
// This menu appears when you right-click empty space
Application.AddDefaultContextMenuExtension(_myMenu);
```

### Example 3: Handling Click Event
```csharp
private void OnMenuClick(object sender, EventArgs e)
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Editor ed = doc.Editor;
    
    // Execute logic
    // Note: Context menu runs in UI thread.
    // If you need to run a command, use SendStringToExecute
    doc.SendStringToExecute("MY_COMMAND ", true, false, false);
}
```

### Example 4: Dynamic Enabling/Disabling
```csharp
_menu.Popup += (s, e) =>
{
    MenuItem item = _menu.MenuItems[0];
    
    // Check condition
    bool valid = CheckSomeCondition();
    
    item.Enabled = valid;
    item.Visible = true; // or hide it
};
```

### Example 5: Submenus
```csharp
MenuItem parentItem = new MenuItem("Advanced Tools");

MenuItem child1 = new MenuItem("Tool A");
MenuItem child2 = new MenuItem("Tool B");

parentItem.MenuItems.Add(child1);
parentItem.MenuItems.Add(child2);

_menu.MenuItems.Add(parentItem);
```

### Example 6: Separators
```csharp
MenuItem separator = new MenuItem(""); 
// Assigning a standard separator style might be required depending on API version
// Or simply adding a separator directly:
_menu.MenuItems.Add(new MenuItem(null)); // Often interpreted as separator
```

### Example 7: Accessing Selected Object
```csharp
// When right-clicking an object, the Valid SelectionSet usually contains it
private void OnEntityClick(object sender, EventArgs e)
{
    Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
    PromptSelectionResult sel = ed.SelectImplied(); // Logic varies
    
    if (sel.Status == PromptStatus.OK)
    {
        // Process selected object
    }
}
```

### Example 8: Removing Menu at Runtime
```csharp
// You can remove menus dynamically if settings change
Application.RemoveObjectContextMenuExtension(...);
```

## Best Practices
1. **Clean Up**: Always remove your extensions in the `Terminate()` method of your `IExtensionApplication`. Failing to do so can leave dead menu items until AutoCAD restarts.
2. **Performance**: Do not perform heavy logic in the `Popup` event check; it delays the menu appearing.
3. **Context**: Remember that the user might have multiple objects selected. Your command should handle `ActiveSelectionSet`.

## Related Objects
- [Application](../Core/Application.md)
- [Entity](../BaseClasses/Entity.md)

## References
- [Autodesk ContextMenuExtension Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Windows_ContextMenuExtension)
