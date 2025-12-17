# PromptResult Class

## Overview
Base class for all prompt result types.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - PromptStatus value
- `StringResult` - String result (for keywords/strings)

## Code Example
```csharp
PromptResult pr = ed.GetString("\nEnter text: ");
if (pr.Status == PromptStatus.OK)
{
    string text = pr.StringResult;
}
```

## Related Classes
- All Prompt result classes

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
