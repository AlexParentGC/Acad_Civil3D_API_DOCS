# PromptAngleResult Class

## Overview
Result of an angle prompt operation.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - Prompt status
- `Value` - Angle value in radians (double)

## Code Example
```csharp
PromptAngleOptions pao = new PromptAngleOptions("\nEnter angle: ");
PromptDoubleResult pdr = ed.GetAngle(pao);
if (pdr.Status == PromptStatus.OK)
{
    double angle = pdr.Value;
    double degrees = angle * 180 / Math.PI;
    ed.WriteMessage($"\nAngle: {degrees}°");
}
```

## Related Classes
- PromptAngleOptions, PromptDoubleResult

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
