# PromptDoubleResult Class

## Overview
Result of a double value prompt.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - Prompt status
- `Value` - Entered double value

## Code Example
```csharp
PromptDoubleResult pdr = ed.GetDouble("\nEnter value: ");
if (pdr.Status == PromptStatus.OK)
{
    double val = pdr.Value;
    ed.WriteMessage($"\nValue: {val}");
}
```

## Related Classes
- PromptDoubleOptions, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
