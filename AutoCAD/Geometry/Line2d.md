# Line2d Struct

## Overview
The `Line2d` struct represents an unbounded line in 2D space (XY plane), defined by a point and a direction vector.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Point` | `Point2d` | Gets a point on the line |
| `Direction` | `Vector2d` | Gets the direction vector of the line |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `Line2d(Point2d, Vector2d)` | Creates line from point and direction |
| `Line2d(Point2d, Point2d)` | Creates line through two points |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Gets closest point on line to given point |
| `DistanceTo(Point2d)` | `double` | Gets distance from point to line |
| `IsOn(Point2d)` | `bool` | Checks if point is on line |
| `IsParallelTo(Line2d)` | `bool` | Checks if parallel to another line |
| `IsPerpendicularTo(Line2d)` | `bool` | Checks if perpendicular to another line |
| `IntersectWith(Line2d)` | `Point2d` | Gets intersection point with another line |

## Code Examples

### Example 1: Creating 2D Lines
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 10);
Line2d line = new Line2d(pt1, pt2);

ed.WriteMessage($"\nLine direction: {line.Direction}");
```

### Example 2: Point-to-Line Distance
```csharp
Line2d line = new Line2d(new Point2d(0, 0), new Point2d(10, 0));
Point2d testPoint = new Point2d(5, 5);

double distance = line.DistanceTo(testPoint);
Point2d closestPoint = line.GetClosestPointTo(testPoint);

ed.WriteMessage($"\nDistance: {distance:F2}");
ed.WriteMessage($"\nClosest point: ({closestPoint.X:F2}, {closestPoint.Y:F2})");
```

## Related Classes
- **LineSegment2d** - Bounded 2D line segment
- **Point2d** - 2D points
- **Vector2d** - 2D direction vector
- **Line3d** - 3D unbounded line

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
