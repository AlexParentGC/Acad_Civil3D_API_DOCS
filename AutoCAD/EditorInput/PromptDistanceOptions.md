# PromptDistanceOptions Class

## Overview
Defines options for prompting the user to enter a distance value.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `BasePoint` - Base point for distance measurement
- `UseBasePoint` - Use base point
- `DefaultValue` - Default distance value
- `AllowNegative` - Allow negative values
- `AllowZero` - Allow zero
- `UseDashedLine` - Show dashed line

## Code Example
```csharp
PromptDistanceOptions pdo = new PromptDistanceOptions("\nEnter distance: ");
pdo.BasePoint = new Point3d(0, 0, 0);
pdo.UseBasePoint = true;
pdo.AllowNegative = false;
PromptDoubleResult pdr = ed.GetDistance(pdo);
if (pdr.Status == PromptStatus.OK)
{
    double distance = pdr.Value;
}
```

## Related Classes
- PromptDoubleResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
