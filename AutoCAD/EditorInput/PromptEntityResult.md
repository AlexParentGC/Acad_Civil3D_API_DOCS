# PromptEntityResult Class

## Overview
Result of an entity selection prompt.

## Namespace
`Autodesk.AutoCAD.EditorInput`

## Key Properties
- `Status` - Prompt status
- `ObjectId` - Selected entity's ObjectId
- `PickedPoint` - Point where entity was picked

## Code Example
```csharp
PromptEntityResult per = ed.GetEntity("\nSelect entity: ");
if (per.Status == PromptStatus.OK)
{
    ObjectId id = per.ObjectId;
    Point3d pickPt = per.PickedPoint;
}
```

## Related Classes
- PromptEntityOptions, ObjectId

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
