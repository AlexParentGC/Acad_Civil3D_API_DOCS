# PromptStringResult Class

## Overview
Result of a string prompt operation.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - Prompt status
- `StringResult` - Entered string value

## Code Example
```csharp
PromptStringOptions pso = new PromptStringOptions("\nEnter name: ");
PromptResult pr = ed.GetString(pso);
if (pr.Status == PromptStatus.OK)
{
    string name = pr.StringResult;
}
```

## Related Classes
- PromptStringOptions, PromptResult

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
