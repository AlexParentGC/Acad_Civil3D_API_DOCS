# PromptDistanceResult Class

## Overview
Result of a distance prompt operation.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - Prompt status
- `Value` - Distance value (double)

## Code Example
```csharp
PromptDistanceOptions pdo = new PromptDistanceOptions("\nEnter distance: ");
pdo.BasePoint = new Point3d(0, 0, 0);
pdo.UseBasePoint = true;
PromptDoubleResult pdr = ed.GetDistance(pdo);
if (pdr.Status == PromptStatus.OK)
{
    double distance = pdr.Value;
    ed.WriteMessage($"\nDistance: {distance}");
}
```

## Related Classes
- PromptDistanceOptions, PromptDoubleResult

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
