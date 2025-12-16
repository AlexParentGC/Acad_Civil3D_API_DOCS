# FeatureLine Class

## Overview
The `FeatureLine` class represents a 3D polyline used for grading and corridor design in Civil3D.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Feature
                  └─ FeatureLine
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Gets/sets the feature line name |
| `Length2D` | `double` | Gets the 2D (horizontal) length |
| `Length3D` | `double` | Gets the 3D (slope) length |
| `MaximumElevation` | `double` | Gets the maximum elevation |
| `MinimumElevation` | `double` | Gets the minimum elevation |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetPoints(FeatureLinePointType)` | `Point3dCollection` | Gets points along the feature line |
| `ElevationAtPoint(Point3d)` | `double` | Gets elevation at a point |

## Code Examples

### Example 1: Accessing Feature Lines
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    ObjectIdCollection featureLineIds = civilDoc.GetFeatureLineIds();
    
    foreach (ObjectId featureLineId in featureLineIds)
    {
        FeatureLine featureLine = tr.GetObject(featureLineId, OpenMode.ForRead) as FeatureLine;
        
        ed.WriteMessage($"\nFeature Line: {featureLine.Name}");
        ed.WriteMessage($"\n  2D Length: {featureLine.Length2D:F2}");
        ed.WriteMessage($"\n  3D Length: {featureLine.Length3D:F2}");
        ed.WriteMessage($"\n  Min Elevation: {featureLine.MinimumElevation:F2}");
        ed.WriteMessage($"\n  Max Elevation: {featureLine.MaximumElevation:F2}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Grading](Grading.md) - Uses feature lines
- [Corridor](../Corridor/Corridor.md) - Can extract feature lines
- [CivilDocument](../Core/CivilDocument.md) - Container for feature lines

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-FeatureLine)
