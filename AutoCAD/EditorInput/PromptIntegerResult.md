# PromptIntegerResult Class

## Overview
Result of an integer value prompt.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - Prompt status
- `Value` - Entered integer value

## Code Example
```csharp
PromptIntegerResult pir = ed.GetInteger("\nEnter count: ");
if (pir.Status == PromptStatus.OK)
{
    int count = pir.Value;
}
```

## Related Classes
- PromptIntegerOptions, Editor

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
