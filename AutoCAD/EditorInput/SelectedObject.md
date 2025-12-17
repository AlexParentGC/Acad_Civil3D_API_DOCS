# SelectedObject Class

## Overview
Represents an object selected by the user during a selection operation.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `ObjectId` - ObjectId of selected entity
- `SelectionMethod` - How object was selected

## Code Example
```csharp
PromptSelectionResult result = ed.GetSelection();
if (result.Status == PromptStatus.OK)
{
    foreach (SelectedObject selObj in result.Value)
    {
        ObjectId id = selObj.ObjectId;
        // Process object
    }
}
```

## Related Classes
- SelectionSet, PromptSelectionResult

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
