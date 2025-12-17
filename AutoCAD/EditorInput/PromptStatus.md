# PromptStatus Enumeration

## Overview
Enumeration defining the status of a prompt operation.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Values

| Value | Description |
|-------|-------------|
| `OK` | User provided valid input |
| `Cancel` | User cancelled (ESC) |
| `Error` | Error occurred |
| `None` | No input provided |
| `Keyword` | Keyword was entered |
| `Modeless` | Modeless operation |
| `Other` | Other status |

## Code Example
```csharp
PromptSelectionResult result = ed.GetSelection();
if (result.Status == PromptStatus.OK)
{
    // Process selection
}
else if (result.Status == PromptStatus.Cancel)
{
    ed.WriteMessage("\nCancelled");
}
```

## Related Classes
- All Prompt result classes

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
