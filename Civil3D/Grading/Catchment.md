# Catchment Class

## Overview
The `Catchment` class represents a drainage catchment area in Civil3D, used for hydrology and drainage analysis.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Catchment
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the catchment name |
| `Area` | `double` | Gets the catchment area |
| `DischargePoint` | `Point3d` | Gets/sets the discharge point location |

## Code Examples

### Example 1: Listing Catchments
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection catchmentIds = civilDoc.GetCatchmentIds();
    
    ed.WriteMessage($"\nFound {catchmentIds.Count} catchments:");
    
    foreach (ObjectId catchmentId in catchmentIds)
    {
        Catchment catchment = tr.GetObject(catchmentId, OpenMode.ForRead) as Catchment;
        
        ed.WriteMessage($"\n  {catchment.Name}");
        ed.WriteMessage($"\n    Area: {catchment.Area:F2} sq units");
        ed.WriteMessage($"\n    Discharge Point: ({catchment.DischargePoint.X:F2}, {catchment.DischargePoint.Y:F2})");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Surface](../Surface/Surface.md) - Used to define catchment boundaries
- [CivilDocument](../Core/CivilDocument.md) - Container for catchments

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-Catchment)
