# Arc Class

## Overview
The `Arc` class represents a circular arc in AutoCAD, defined by a center point, radius, start angle, and end angle.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Arc
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point3d` | Gets/sets the center point |
| `Radius` | `double` | Gets/sets the radius |
| `StartAngle` | `double` | Gets/sets the start angle (radians) |
| `EndAngle` | `double` | Gets/sets the end angle (radians) |
| `TotalAngle` | `double` | Gets the total angle swept (radians) |
| `StartPoint` | `Point3d` | Gets the start point |
| `EndPoint` | `Point3d` | Gets the end point |
| `Length` | `double` | Gets the arc length |
| `Normal` | `Vector3d` | Gets/sets the normal vector |
| `Thickness` | `double` | Gets/sets the thickness |

## Code Examples

### Example 1: Creating an Arc
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create an arc: center (50,50), radius 25, from 0° to 90°
    Arc arc = new Arc();
    arc.Center = new Point3d(50, 50, 0);
    arc.Radius = 25.0;
    arc.StartAngle = 0; // 0 degrees
    arc.EndAngle = Math.PI / 2; // 90 degrees
    
    btr.AppendEntity(arc);
    tr.AddNewlyCreatedDBObject(arc, true);
    
    tr.Commit();
}
```

### Example 2: Creating Arc from 3 Points
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d startPt = new Point3d(0, 0, 0);
    Point3d midPt = new Point3d(50, 50, 0);
    Point3d endPt = new Point3d(100, 0, 0);
    
    // Use CircularArc3d to calculate arc parameters
    CircularArc3d arc3d = new CircularArc3d(startPt, midPt, endPt);
    
    Arc arc = new Arc();
    arc.Center = arc3d.Center;
    arc.Radius = arc3d.Radius;
    arc.StartAngle = arc3d.ReferenceVector.AngleOnPlane(new Plane(arc3d.Center, arc3d.Normal));
    arc.EndAngle = arc.StartAngle + (arc3d.EndAngle - arc3d.StartAngle);
    arc.Normal = arc3d.Normal;
    
    btr.AppendEntity(arc);
    tr.AddNewlyCreatedDBObject(arc, true);
    
    tr.Commit();
}
```

### Example 3: Getting Arc Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Arc arc = tr.GetObject(arcId, OpenMode.ForRead) as Arc;
    
    double startAngleDeg = arc.StartAngle * (180.0 / Math.PI);
    double endAngleDeg = arc.EndAngle * (180.0 / Math.PI);
    double totalAngleDeg = arc.TotalAngle * (180.0 / Math.PI);
    double arcLength = arc.Length;
    
    ed.WriteMessage($"\nStart Angle: {startAngleDeg:F2}°");
    ed.WriteMessage($"\nEnd Angle: {endAngleDeg:F2}°");
    ed.WriteMessage($"\nTotal Angle: {totalAngleDeg:F2}°");
    ed.WriteMessage($"\nArc Length: {arcLength:F2}");
    
    tr.Commit();
}
```

## Related Objects
- [Circle](Circle.md) - Full circle
- [Ellipse](Ellipse.md) - Elliptical arc
- [Curve](Curve.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Arc)
