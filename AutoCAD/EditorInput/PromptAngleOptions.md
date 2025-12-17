# PromptAngleOptions Class

## Overview
Defines options for prompting the user to enter an angle value.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Message` - Prompt message
- `BasePoint` - Base point for angle measurement
- `UseBasePoint` - Use base point
- `DefaultValue` - Default angle value
- `UseAngleBase` - Use angle base setting
- `UseDashedLine` - Show dashed line

## Code Example
```csharp
PromptAngleOptions pao = new PromptAngleOptions("\nEnter angle: ");
pao.BasePoint = new Point3d(0, 0, 0);
pao.UseBasePoint = true;
PromptDoubleResult pdr = ed.GetAngle(pao);
if (pdr.Status == PromptStatus.OK)
{
    double angle = pdr.Value;
}
```

## Related Classes
- PromptDoubleResult, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
