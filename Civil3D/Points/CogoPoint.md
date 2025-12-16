# CogoPoint Class

## Overview
The `CogoPoint` class represents a coordinate geometry (COGO) point in Civil3D, used for survey points and site layout.

## Namespace
`Autodesk.Civil.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ CogoPoint
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `PointNumber` | `uint` | Gets/sets the point number |
| `Easting` | `double` | Gets/sets the X coordinate (Easting) |
| `Northing` | `double` | Gets/sets the Y coordinate (Northing) |
| `Elevation` | `double` | Gets/sets the Z coordinate (Elevation) |
| `RawDescription` | `string` | Gets/sets the point description |
| `FullDescription` | `string` | Gets the full description with codes |
| `Location` | `Point3d` | Gets/sets the 3D location |

## Code Examples

### Example 1: Accessing COGO Points
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    CogoPointCollection cogoPoints = civilDoc.CogoPoints;
    ObjectIdCollection pointIds = cogoPoints.GetPointIds();
    
    foreach (ObjectId pointId in pointIds)
    {
        CogoPoint point = tr.GetObject(pointId, OpenMode.ForRead) as CogoPoint;
        
        ed.WriteMessage($"\nPoint {point.PointNumber}:");
        ed.WriteMessage($"\n  E: {point.Easting:F3}");
        ed.WriteMessage($"\n  N: {point.Northing:F3}");
        ed.WriteMessage($"\n  Elev: {point.Elevation:F3}");
        ed.WriteMessage($"\n  Desc: {point.RawDescription}");
    }
    
    tr.Commit();
}
```

### Example 2: Creating a COGO Point
```csharp
using (Transaction tr = civilDoc.Database.TransactionManager.StartTransaction())
{
    CogoPointCollection cogoPoints = civilDoc.CogoPoints;
    
    // Add a new point
    ObjectId pointId = cogoPoints.Add(new Point3d(1000, 2000, 150), true);
    
    CogoPoint newPoint = tr.GetObject(pointId, OpenMode.ForWrite) as CogoPoint;
    newPoint.PointNumber = 100;
    newPoint.RawDescription = "Property Corner";
    
    tr.Commit();
}
```

## Related Objects
- [CivilDocument](../Core/CivilDocument.md) - Contains CogoPointCollection
- [Surface](../Surface/Surface.md) - Can be created from COGO points

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-CogoPoint)
