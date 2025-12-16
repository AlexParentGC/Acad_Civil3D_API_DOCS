# Grading Class

## Overview
The `Grading` class represents a grading object in Civil3D, used for site grading and earthwork design.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Grading
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets the grading name |
| `SurfaceId` | `ObjectId` | Gets/sets the target surface |
| `StyleId` | `ObjectId` | Gets/sets the grading style |

## Code Examples

### Example 1: Listing Gradings
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection gradingIds = civilDoc.GetGradingIds();
    
    ed.WriteMessage($"\nFound {gradingIds.Count} grading objects:");
    
    foreach (ObjectId gradingId in gradingIds)
    {
        Grading grading = tr.GetObject(gradingId, OpenMode.ForRead) as Grading;
        
        ed.WriteMessage($"\n  {grading.Name}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Surface](../Surface/Surface.md) - Target surface for grading
- [FeatureLine](FeatureLine.md) - Often used with grading
- [CivilDocument](../Core/CivilDocument.md) - Container for gradings

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Grading)
