# LineSegment2d Struct

## Overview
The `LineSegment2d` struct represents a bounded line segment in 2D space (XY plane), defined by start and end points.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `StartPoint` | `Point2d` | Gets the start point of the segment |
| `EndPoint` | `Point2d` | Gets the end point of the segment |
| `MidPoint` | `Point2d` | Gets the midpoint of the segment |
| `Direction` | `Vector2d` | Gets the direction vector from start to end |
| `Length` | `double` | Gets the length of the segment |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `LineSegment2d(Point2d, Point2d)` | Creates segment from start and end points |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Gets closest point on segment to given point |
| `DistanceTo(Point2d)` | `double` | Gets distance from point to segment |
| `IsOn(Point2d)` | `bool` | Checks if point is on segment |
| `IsParallelTo(LineSegment2d)` | `bool` | Checks if parallel to another segment |
| `IsPerpendicularTo(LineSegment2d)` | `bool` | Checks if perpendicular to another segment |

## Code Examples

### Example 1: Creating 2D Line Segments
```csharp
Point2d start = new Point2d(0, 0);
Point2d end = new Point2d(10, 10);
LineSegment2d segment = new LineSegment2d(start, end);

ed.WriteMessage($"\nLength: {segment.Length:F2}");
ed.WriteMessage($"\nMidpoint: ({segment.MidPoint.X:F2}, {segment.MidPoint.Y:F2})");
```

### Example 2: Distance Calculation
```csharp
LineSegment2d segment = new LineSegment2d(
    new Point2d(0, 0),
    new Point2d(10, 0)
);

Point2d testPoint = new Point2d(5, 5);
double distance = segment.DistanceTo(testPoint);

ed.WriteMessage($"\nDistance: {distance:F2}");
```

## Related Classes
- **LineSegment3d** - 3D line segment
- **Line2d** - Unbounded 2D line
- **Point2d** - 2D points
- **Vector2d** - 2D direction vector

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
