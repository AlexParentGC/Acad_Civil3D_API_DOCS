# Ellipse Class

## Overview
The `Ellipse` class represents an elliptical shape in AutoCAD, defined by a center point, major and minor axes.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Ellipse
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point3d` | Gets/sets the center point |
| `MajorAxis` | `Vector3d` | Gets/sets the major axis vector |
| `MinorAxis` | `Vector3d` | Gets the minor axis vector (read-only) |
| `MajorRadius` | `double` | Gets/sets the major radius |
| `MinorRadius` | `double` | Gets/sets the minor radius |
| `RadiusRatio` | `double` | Gets/sets the ratio of minor to major radius |
| `StartAngle` | `double` | Gets/sets the start angle (for elliptical arcs) |
| `EndAngle` | `double` | Gets/sets the end angle (for elliptical arcs) |
| `Normal` | `Vector3d` | Gets/sets the normal vector |

## Code Examples

### Example 1: Creating an Ellipse
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d center = new Point3d(100, 100, 0);
    Vector3d majorAxis = new Vector3d(50, 0, 0); // 50 units in X direction
    Vector3d normal = Vector3d.ZAxis;
    double radiusRatio = 0.5; // Minor radius is half of major
    
    Ellipse ellipse = new Ellipse(center, normal, majorAxis, radiusRatio, 0, 2 * Math.PI);
    
    btr.AppendEntity(ellipse);
    tr.AddNewlyCreatedDBObject(ellipse, true);
    
    tr.Commit();
}
```

### Example 2: Creating an Elliptical Arc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d center = new Point3d(200, 200, 0);
    Vector3d majorAxis = new Vector3d(40, 0, 0);
    Vector3d normal = Vector3d.ZAxis;
    double radiusRatio = 0.6;
    double startAngle = 0;
    double endAngle = Math.PI; // Half ellipse
    
    Ellipse ellipseArc = new Ellipse(center, normal, majorAxis, radiusRatio, startAngle, endAngle);
    
    btr.AppendEntity(ellipseArc);
    tr.AddNewlyCreatedDBObject(ellipseArc, true);
    
    tr.Commit();
}
```

## Related Objects
- [Circle](Circle.md) - Special case where radiusRatio = 1
- [Arc](Arc.md) - Circular arc
- [Curve](../../BaseClasses/Curve.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Ellipse)
