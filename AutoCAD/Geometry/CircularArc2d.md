# CircularArc2d Struct

## Overview
The `CircularArc2d` struct represents a circular arc in 2D space (XY plane), defined by center, radius, and start/end angles.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point2d` | Gets the center point of the arc |
| `Radius` | `double` | Gets the radius of the arc |
| `StartAngle` | `double` | Gets the start angle (radians) |
| `EndAngle` | `double` | Gets the end angle (radians) |
| `StartPoint` | `Point2d` | Gets the start point of the arc |
| `EndPoint` | `Point2d` | Gets the end point of the arc |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `CircularArc2d(Point2d, double)` | Creates full circle |
| `CircularArc2d(Point2d, double, double, double)` | Creates arc with start/end angles |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Gets closest point on arc to given point |
| `DistanceTo(Point2d)` | `double` | Gets distance from point to arc |
| `IsOn(Point2d)` | `bool` | Checks if point is on arc |

## Code Examples

### Example 1: Creating 2D Arcs
```csharp
Point2d center = new Point2d(0, 0);
double radius = 10.0;
double startAngle = 0;
double endAngle = Math.PI / 2; // 90 degrees

CircularArc2d arc = new CircularArc2d(center, radius, startAngle, endAngle);

ed.WriteMessage($"\nArc radius: {arc.Radius}");
ed.WriteMessage($"\nStart point: ({arc.StartPoint.X:F2}, {arc.StartPoint.Y:F2})");
ed.WriteMessage($"\nEnd point: ({arc.EndPoint.X:F2}, {arc.EndPoint.Y:F2})");
```

## Related Classes
- **CircularArc3d** - 3D circular arc
- **Point2d** - 2D center and points
- **Arc** - AutoCAD arc entity

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
