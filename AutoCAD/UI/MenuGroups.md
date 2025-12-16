# MenuGroups Class

## Overview
The `MenuGroups` class represents the collection of menu groups loaded in AutoCAD. Menu groups are associated with CUIx files and contain menus, toolbars, and other UI elements.

## Namespace
`Autodesk.AutoCAD.Windows` (COM object)

## Code Examples

### Example 1: Listing Menu Groups
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuGroups = Application.MenuGroups;

ed.WriteMessage($"\nTotal Menu Groups: {menuGroups.Count}");
ed.WriteMessage("\nMenu Groups:");

for (int i = 0; i < menuGroups.Count; i++)
{
    dynamic menuGroup = menuGroups.Item(i);
    ed.WriteMessage($"\n  {menuGroup.Name}");
}
```

### Example 2: Finding a Specific Menu Group
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuGroups = Application.MenuGroups;

string targetGroup = "ACAD";

for (int i = 0; i < menuGroups.Count; i++)
{
    dynamic menuGroup = menuGroups.Item(i);
    
    if (menuGroup.Name == targetGroup)
    {
        ed.WriteMessage($"\nFound menu group: {menuGroup.Name}");
        
        // Access menus in this group
        dynamic menus = menuGroup.Menus;
        ed.WriteMessage($"\n  Menus in group: {menus.Count}");
        break;
    }
}
```

### Example 3: Loading a Menu Group
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic menuGroups = Application.MenuGroups;

// Load a CUIx file
string cuixPath = "C:\\MyCustom\\MyMenu.cuix";

try
{
    dynamic newGroup = menuGroups.Load(cuixPath);
    ed.WriteMessage($"\nLoaded menu group: {newGroup.Name}");
}
catch (System.Exception ex)
{
    ed.WriteMessage($"\nError loading menu group: {ex.Message}");
}
```

## Related Objects
- [Application](Application.md) - Provides access to MenuGroups
- [MenuBar](MenuBar.md) - Menu bar

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
