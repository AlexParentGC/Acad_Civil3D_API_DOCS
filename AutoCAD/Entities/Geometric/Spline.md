# Spline Class

## Overview
The `Spline` class represents a smooth curve defined by control points and fit points in AutoCAD.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Spline
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Degree` | `int` | Gets the degree of the spline |
| `Rational` | `bool` | Gets whether the spline is rational (NURBS) |
| `Closed` | `bool` | Gets/sets whether the spline is closed |
| `Periodic` | `bool` | Gets whether the spline is periodic |
| `NumControlPoints` | `int` | Gets the number of control points |
| `NumFitPoints` | `int` | Gets the number of fit points |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `SetControlPointAt(int, Point3d)` | `void` | Sets a control point |
| `GetControlPointAt(int)` | `Point3d` | Gets a control point |
| `SetFitPointAt(int, Point3d)` | `void` | Sets a fit point |
| `GetFitPointAt(int)` | `Point3d` | Gets a fit point |

## Code Examples

### Example 1: Creating a Spline from Fit Points
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3dCollection fitPoints = new Point3dCollection();
    fitPoints.Add(new Point3d(0, 0, 0));
    fitPoints.Add(new Point3d(50, 50, 0));
    fitPoints.Add(new Point3d(100, 25, 0));
    fitPoints.Add(new Point3d(150, 75, 0));
    fitPoints.Add(new Point3d(200, 50, 0));
    
    Spline spline = new Spline(fitPoints, 3, 0.0); // Degree 3, tolerance 0
    
    btr.AppendEntity(spline);
    tr.AddNewlyCreatedDBObject(spline, true);
    
    tr.Commit();
}
```

## Related Objects
- [Polyline](../Polylines/Polyline.md) - Multi-segment line
- [Curve](../../BaseClasses/Curve.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Spline)
