# SelectedSubObject Class

## Overview
Represents a selected sub-entity (face, edge, vertex) of a solid or surface.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `ObjectId` - Parent object ID
- `FullSubentityPath` - Path to sub-entity
- `GraphicsSystemMarker` - Graphics marker

## Code Example
```csharp
// Sub-entity selection for solids/surfaces
PromptSelectionResult result = ed.GetSelection();
if (result.Status == PromptStatus.OK)
{
    foreach (SelectedObject selObj in result.Value)
    {
        if (selObj is SelectedSubObject subObj)
        {
            // Process sub-entity
        }
    }
}
```

## Related Classes
- SelectedObject, FullSubentityPath

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
