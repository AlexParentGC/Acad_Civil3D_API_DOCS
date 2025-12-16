# MenuBar Class

## Overview
The `MenuBar` class provides access to AutoCAD's menu bar for customization and interaction. Note that this is a COM object from the AutoCAD ActiveX Automation library.

## Namespace
`Autodesk.AutoCAD.Windows` (COM object)

## Code Examples

### Example 1: Accessing Menu Bar
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuBar = Application.MenuBar;

// Get menu bar properties
int menuCount = menuBar.Count;
ed.WriteMessage($"\nNumber of menus: {menuCount}");
```

### Example 2: Iterating Through Menus
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuBar = Application.MenuBar;

ed.WriteMessage("\nMenu Bar Items:");
for (int i = 0; i < menuBar.Count; i++)
{
    dynamic menu = menuBar.Item(i);
    ed.WriteMessage($"\n  {menu.Name}");
}
```

## Related Objects
- [Application](Application.md) - Provides access to MenuBar
- [MenuGroups](MenuGroups.md) - Menu groups collection

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
