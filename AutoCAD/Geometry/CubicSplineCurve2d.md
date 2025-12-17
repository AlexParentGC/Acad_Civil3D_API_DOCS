# CubicSplineCurve2d Class

## Overview
The `CubicSplineCurve2d` class represents an interpolation cubic spline in 2D space (XY plane).

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `FitPoints` | `Point2dCollection` | Gets the fit points |
| `StartTangent` | `Vector2d` | Gets start tangent |
| `EndTangent` | `Vector2d` | Gets end tangent |

## Code Examples

### Example 1: Creating 2D Cubic Spline
```csharp
Point2dCollection fitPoints = new Point2dCollection();
fitPoints.Add(new Point2d(0, 0));
fitPoints.Add(new Point2d(5, 5));
fitPoints.Add(new Point2d(10, 3));
fitPoints.Add(new Point2d(15, 7));

CubicSplineCurve2d spline = new CubicSplineCurve2d(fitPoints);

ed.WriteMessage($"\n2D cubic spline created");
```

## Related Classes
- **CubicSplineCurve3d** - 3D cubic spline
- **NurbCurve2d** - 2D NURBS curve

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
