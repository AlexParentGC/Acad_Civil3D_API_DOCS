# PromptPointResult Class

## Overview
Result of a point selection prompt.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - Prompt status (OK, Cancel, Error)
- `Value` - Selected Point3d

## Code Example
```csharp
PromptPointResult ppr = ed.GetPoint("\nSelect point: ");
if (ppr.Status == PromptStatus.OK)
{
    Point3d pt = ppr.Value;
    ed.WriteMessage($"\nPoint: ({pt.X}, {pt.Y}, {pt.Z})");
}
```

## Related Classes
- PromptPointOptions, Point3d

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
