# EntityColor Class

## Overview
Represents entity color properties in AutoCAD.

## Namespace
`Autodesk.AutoCAD.Colors`

## Key Properties
- `ColorMethod` - Color method
- `ColorIndex` - ACI color index
- `Red`, `Green`, `Blue` - RGB components

## Code Example
```csharp
using Autodesk.AutoCAD.Colors;
using Autodesk.AutoCAD.DatabaseServices;

Entity ent = /* get entity */;
EntityColor ec = ent.EntityColor;

if (ec.IsByLayer)
{
    ed.WriteMessage("\nColor is ByLayer");
}
else if (ec.IsByAci)
{
    ed.WriteMessage($"\nACI: {ec.ColorIndex}");
}
```

## Related Classes
- Color, ColorMethod

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
