# Polyline3d Class

## Overview
The `Polyline3d` class represents a piecewise linear spline entity in 3D space. Unlike smooth splines, polylines consist of straight line segments connecting vertices.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Vertices` | `Point3dCollection` | Gets the vertices of the polyline |
| `NumVertices` | `int` | Gets the number of vertices |
| `IsClosed` | `bool` | Checks if polyline is closed |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on polyline |
| `GetSegmentAt(int)` | `LineSegment3d` | Gets line segment at index |
| `Length` | `double` | Gets total length of polyline |

## Code Examples

### Example 1: Creating 3D Polyline
```csharp
Point3dCollection vertices = new Point3dCollection();
vertices.Add(new Point3d(0, 0, 0));
vertices.Add(new Point3d(10, 0, 0));
vertices.Add(new Point3d(10, 10, 0));
vertices.Add(new Point3d(0, 10, 5));

Polyline3d polyline = new Polyline3d(vertices, false); // not closed

ed.WriteMessage($"\n3D Polyline with {polyline.NumVertices} vertices");
```

### Example 2: Closed Polyline
```csharp
Point3dCollection vertices = new Point3dCollection();
vertices.Add(new Point3d(0, 0, 0));
vertices.Add(new Point3d(10, 0, 0));
vertices.Add(new Point3d(10, 10, 0));
vertices.Add(new Point3d(0, 10, 0));

Polyline3d closedPolyline = new Polyline3d(vertices, true); // closed

ed.WriteMessage($"\nClosed polyline: {closedPolyline.IsClosed}");
```

## Best Practices

1. **Piecewise Linear**: Straight segments, not smooth
2. **Closed**: Set to true for closed shapes
3. **Vertices**: Minimum 2 vertices required
4. **Approximation**: Use to approximate smooth curves

## Related Classes
- **Polyline2d** - 2D polyline
- **LineSegment3d** - Individual segments
- **CubicSplineCurve3d** - Smooth alternative

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Polyline3d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Polyline3d)
