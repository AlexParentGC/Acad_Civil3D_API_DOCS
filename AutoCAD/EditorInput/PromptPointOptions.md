# PromptPointOptions Class

## Overview
Defines options for prompting the user to select a point in AutoCAD.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `BasePoint` - Base point for relative input
- `UseBasePoint` - Use base point
- `UseDashedLine` - Show dashed line from base point
- `Keywords` - Available keywords
- `AllowNone` - Allow null response
- `AllowArbitraryInput` - Allow arbitrary input

## Code Example
```csharp
PromptPointOptions ppo = new PromptPointOptions("\nSelect point: ");
ppo.AllowNone = false;
PromptPointResult ppr = ed.GetPoint(ppo);
if (ppr.Status == PromptStatus.OK)
{
    Point3d point = ppr.Value;
}
```

## Related Classes
- PromptPointResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
