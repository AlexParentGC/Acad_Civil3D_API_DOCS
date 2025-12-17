# Polyline2d Class

## Overview
The `Polyline2d` class represents a piecewise linear spline entity in 2D space (XY plane).

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Vertices` | `Point2dCollection` | Gets the vertices |
| `NumVertices` | `int` | Gets number of vertices |
| `IsClosed` | `bool` | Checks if closed |

## Code Examples

### Example 1: Creating 2D Polyline
```csharp
Point2dCollection vertices = new Point2dCollection();
vertices.Add(new Point2d(0, 0));
vertices.Add(new Point2d(10, 0));
vertices.Add(new Point2d(10, 10));
vertices.Add(new Point2d(0, 10));

Polyline2d polyline = new Polyline2d(vertices, true); // closed

ed.WriteMessage($"\n2D Polyline: {polyline.NumVertices} vertices, closed={polyline.IsClosed}");
```

## Related Classes
- **Polyline3d** - 3D polyline
- **LineSegment2d** - Individual segments

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
