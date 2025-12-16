# InfoCenter Class

## Overview
The `InfoCenter` class provides access to AutoCAD's InfoCenter, which is the search and help system in the application title bar.

## Namespace
`Autodesk.AutoCAD.ApplicationServices`

## Code Examples

### Example 1: Accessing InfoCenter
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic infoCenter = Application.InfoCenter;

// Access InfoCenter for search and help functionality
ed.WriteMessage("\nInfoCenter accessed");
```

### Example 2: InfoCenter Integration (Conceptual)
```csharp
using Autodesk.AutoCAD.ApplicationServices;

dynamic infoCenter = Application.InfoCenter;

// InfoCenter can be used to integrate custom help
// and search functionality into AutoCAD's help system

ed.WriteMessage("\nInfoCenter integration available");
```

## Related Objects
- [Application](Application.md) - Provides access to InfoCenter

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
