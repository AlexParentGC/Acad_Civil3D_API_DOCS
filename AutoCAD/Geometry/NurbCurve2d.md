# NurbCurve2d Class

## Overview
The `NurbCurve2d` class represents a non-uniform rational B-spline (NURBS) curve in 2D space (XY plane). Like its 3D counterpart, it provides precise mathematical representation of complex curves using control points, knots, and weights.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ControlPoints` | `Point2dCollection` | Gets the control points that define the curve shape |
| `Knots` | `KnotCollection` | Gets the knot vector |
| `Weights` | `DoubleCollection` | Gets the weights for rational NURBS |
| `Degree` | `int` | Gets the polynomial degree of the curve |
| `NumControlPoints` | `int` | Gets the number of control points |
| `IsRational` | `bool` | Checks if the curve has weights |
| `IsPeriodic` | `bool` | Checks if the curve is periodic (closed) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `EvaluatePoint(double)` | `Point2d` | Evaluates point on curve at parameter |
| `GetClosestPointTo(Point2d)` | `Point2d` | Gets closest point on curve |
| `GetDistanceTo(Point2d)` | `double` | Gets distance from point to curve |
| `GetDerivativeAt(double)` | `Vector2d` | Gets tangent at parameter |

## Code Examples

### Example 1: Creating 2D NURBS Curve
```csharp
Point2dCollection controlPoints = new Point2dCollection();
controlPoints.Add(new Point2d(0, 0));
controlPoints.Add(new Point2d(10, 5));
controlPoints.Add(new Point2d(20, 5));
controlPoints.Add(new Point2d(30, 0));

DoubleCollection knots = new DoubleCollection();
knots.Add(0); knots.Add(0); knots.Add(0); knots.Add(0);
knots.Add(1); knots.Add(1); knots.Add(1); knots.Add(1);

NurbCurve2d curve = new NurbCurve2d(3, knots, controlPoints, false);

ed.WriteMessage($"\n2D NURBS created with {curve.NumControlPoints} control points");
```

### Example 2: Evaluating 2D NURBS
```csharp
NurbCurve2d curve = /* existing curve */;

for (int i = 0; i <= 10; i++)
{
    double param = i / 10.0;
    Point2d pt = curve.EvaluatePoint(param);
    Vector2d tangent = curve.GetDerivativeAt(param);
    
    ed.WriteMessage($"\nParam {param:F1}: ({pt.X:F2}, {pt.Y:F2}), Tangent: ({tangent.X:F2}, {tangent.Y:F2})");
}
```

## Best Practices

1. **2D Operations**: Use NurbCurve2d for planar geometry
2. **Knot Vector**: Same rules as 3D (length = numControlPoints + degree + 1)
3. **Performance**: 2D NURBS are faster than 3D for planar work

## Related Classes
- **NurbCurve3d** - 3D NURBS curve
- **CubicSplineCurve2d** - 2D cubic spline
- **Point2d** - Control points
- **Vector2d** - Tangent vectors

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [NurbCurve2d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_NurbCurve2d)
