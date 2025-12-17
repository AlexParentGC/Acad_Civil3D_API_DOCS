# CubicSplineCurve3d Class

## Overview
The `CubicSplineCurve3d` class represents an interpolation cubic spline in 3D space. Cubic splines create smooth curves that pass through specified fit points, commonly used for smooth interpolation.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `FitPoints` | `Point3dCollection` | Gets the points the curve passes through |
| `StartTangent` | `Vector3d` | Gets the tangent vector at the start |
| `EndTangent` | `Vector3d` | Gets the tangent vector at the end |
| `NumFitPoints` | `int` | Gets the number of fit points |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `EvaluatePoint(double)` | `Point3d` | Evaluates point on curve at parameter |
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on curve |
| `GetDerivativeAt(double)` | `Vector3d` | Gets tangent at parameter |

## Code Examples

### Example 1: Creating Cubic Spline
```csharp
// Create spline through fit points
Point3dCollection fitPoints = new Point3dCollection();
fitPoints.Add(new Point3d(0, 0, 0));
fitPoints.Add(new Point3d(5, 5, 0));
fitPoints.Add(new Point3d(10, 3, 0));
fitPoints.Add(new Point3d(15, 7, 0));
fitPoints.Add(new Point3d(20, 5, 0));

CubicSplineCurve3d spline = new CubicSplineCurve3d(fitPoints);

ed.WriteMessage($"\nCubic spline created through {spline.NumFitPoints} points");
```

### Example 2: Spline with Tangents
```csharp
Point3dCollection fitPoints = new Point3dCollection();
fitPoints.Add(new Point3d(0, 0, 0));
fitPoints.Add(new Point3d(10, 10, 0));
fitPoints.Add(new Point3d(20, 0, 0));

// Specify start and end tangents
Vector3d startTangent = Vector3d.XAxis;
Vector3d endTangent = Vector3d.XAxis;

CubicSplineCurve3d spline = new CubicSplineCurve3d(fitPoints, startTangent, endTangent);

ed.WriteMessage($"\nSpline with specified tangents");
ed.WriteMessage($"\nStart tangent: {spline.StartTangent}");
ed.WriteMessage($"\nEnd tangent: {spline.EndTangent}");
```

## Best Practices

1. **Fit Points**: Curve passes through all fit points
2. **Smoothness**: Cubic splines provide C² continuity (smooth)
3. **Tangents**: Specify tangents for better control at endpoints
4. **Interpolation**: Use for smooth curves through known points

## Related Classes
- **CubicSplineCurve2d** - 2D cubic spline
- **NurbCurve3d** - NURBS curve (more control)
- **Polyline3d** - Piecewise linear (not smooth)

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [CubicSplineCurve3d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_CubicSplineCurve3d)
