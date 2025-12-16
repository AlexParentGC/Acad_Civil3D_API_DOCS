# Curve Class

## Overview
The `Curve` class is the base class for all curve-based entities in AutoCAD including lines, arcs, circles, polylines, ellipses, and splines. It provides common curve properties and methods.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  ├─ Line
                  ├─ Arc
                  ├─ Circle
                  ├─ Ellipse
                  ├─ Spline
                  ├─ Polyline
                  ├─ Polyline2d
                  └─ Polyline3d
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `StartPoint` | `Point3d` | Gets the start point of the curve |
| `EndPoint` | `Point3d` | Gets the end point of the curve |
| `StartParam` | `double` | Gets the start parameter value |
| `EndParam` | `double` | Gets the end parameter value |
| `Closed` | `bool` | Gets whether the curve is closed |
| `Periodic` | `bool` | Gets whether the curve is periodic |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetPointAtParameter(double)` | `Point3d` | Gets point at parameter value |
| `GetParameterAtPoint(Point3d)` | `double` | Gets parameter at point |
| `GetDistAtParam(double)` | `double` | Gets distance along curve at parameter |
| `GetParamAtDist(double)` | `double` | Gets parameter at distance |
| `GetPointAtDist(double)` | `Point3d` | Gets point at distance along curve |
| `GetDistAtPoint(Point3d)` | `double` | Gets distance along curve to point |
| `GetClosestPointTo(Point3d, bool)` | `Point3d` | Gets closest point on curve |
| `GetFirstDerivative(double)` | `Vector3d` | Gets tangent vector at parameter |
| `GetSecondDerivative(double)` | `Vector3d` | Gets curvature vector at parameter |
| `GetSplitCurves(Point3dCollection)` | `DBObjectCollection` | Splits curve at points |
| `Extend(double)` | `void` | Extends curve to parameter |
| `Extend(bool, Point3d)` | `void` | Extends curve to point |

## Code Examples

### Example 1: Getting Points Along a Curve
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    
    // Get points at regular intervals
    double totalLength = curve.GetDistAtParam(curve.EndParam);
    int numPoints = 10;
    
    for (int i = 0; i <= numPoints; i++)
    {
        double distance = (totalLength / numPoints) * i;
        Point3d point = curve.GetPointAtDist(distance);
        
        ed.WriteMessage($"\nPoint {i}: ({point.X:F2}, {point.Y:F2})");
    }
    
    tr.Commit();
}
```

### Example 2: Finding Closest Point on Curve
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    
    Point3d testPoint = new Point3d(100, 100, 0);
    
    // Get closest point (extend = false means don't extend beyond endpoints)
    Point3d closestPoint = curve.GetClosestPointTo(testPoint, false);
    
    double distance = testPoint.DistanceTo(closestPoint);
    
    ed.WriteMessage($"\nClosest point: ({closestPoint.X:F2}, {closestPoint.Y:F2})");
    ed.WriteMessage($"\nDistance: {distance:F2}");
    
    tr.Commit();
}
```

### Example 3: Getting Tangent Direction
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    
    // Get tangent at midpoint
    double midParam = (curve.StartParam + curve.EndParam) / 2;
    Vector3d tangent = curve.GetFirstDerivative(midParam);
    
    // Normalize to unit vector
    tangent = tangent.GetNormal();
    
    double angle = Math.Atan2(tangent.Y, tangent.X) * (180.0 / Math.PI);
    
    ed.WriteMessage($"\nTangent angle at midpoint: {angle:F2}°");
    
    tr.Commit();
}
```

### Example 4: Splitting a Curve
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Curve curve = tr.GetObject(curveId, OpenMode.ForRead) as Curve;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create split points
    Point3dCollection splitPoints = new Point3dCollection();
    splitPoints.Add(curve.GetPointAtDist(curve.GetDistAtParam(curve.EndParam) * 0.33));
    splitPoints.Add(curve.GetPointAtDist(curve.GetDistAtParam(curve.EndParam) * 0.66));
    
    // Split the curve
    DBObjectCollection splitCurves = curve.GetSplitCurves(splitPoints);
    
    // Add split curves to database
    foreach (DBObject obj in splitCurves)
    {
        Curve splitCurve = obj as Curve;
        btr.AppendEntity(splitCurve);
        tr.AddNewlyCreatedDBObject(splitCurve, true);
    }
    
    tr.Commit();
}
```

## Related Objects
- [Entity](Entity.md) - Base class
- [Line](../Entities/Geometric/Line.md) - Derived class
- [Arc](../Entities/Geometric/Arc.md) - Derived class
- [Circle](../Entities/Geometric/Circle.md) - Derived class
- [Polyline](../Entities/Polylines/Polyline.md) - Derived class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Curve)
