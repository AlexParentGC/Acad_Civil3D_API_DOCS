# Line3d Struct

## Overview
The `Line3d` struct represents an unbounded line in 3D space, defined by a point and a direction vector. Unlike the `Line` entity class, `Line3d` extends infinitely in both directions.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Point` | `Point3d` | Gets a point on the line |
| `Direction` | `Vector3d` | Gets the direction vector of the line |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `Line3d(Point3d, Vector3d)` | Creates line from point and direction |
| `Line3d(Point3d, Point3d)` | Creates line through two points |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on line to given point |
| `GetClosestPointTo(Line3d)` | `Point3d` | Gets closest point to another line |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to line |
| `DistanceTo(Line3d)` | `double` | Gets distance between two lines |
| `IsOn(Point3d)` | `bool` | Checks if point is on line |
| `IsParallelTo(Line3d)` | `bool` | Checks if parallel to another line |
| `IsPerpendicularTo(Line3d)` | `bool` | Checks if perpendicular to another line |
| `IsColinearTo(Line3d)` | `bool` | Checks if colinear with another line |
| `IntersectWith(Line3d)` | `Point3d` | Gets intersection point with another line |
| `IntersectWith(Plane)` | `Point3d` | Gets intersection point with plane |

## Code Examples

### Example 1: Creating Lines
```csharp
// From point and direction
Point3d pt = new Point3d(0, 0, 0);
Vector3d dir = new Vector3d(1, 1, 0).GetNormal();
Line3d line1 = new Line3d(pt, dir);

// From two points
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 10, 0);
Line3d line2 = new Line3d(pt1, pt2);

ed.WriteMessage($"\nLine direction: {line2.Direction}");
```

### Example 2: Point-to-Line Distance
```csharp
Line3d line = new Line3d(new Point3d(0, 0, 0), new Point3d(10, 0, 0));
Point3d testPoint = new Point3d(5, 5, 0);

double distance = line.DistanceTo(testPoint);
Point3d closestPoint = line.GetClosestPointTo(testPoint);

ed.WriteMessage($"\nDistance to line: {distance:F2}");
ed.WriteMessage($"\nClosest point: {closestPoint}");
```

### Example 3: Line Intersection
```csharp
// Two intersecting lines
Line3d line1 = new Line3d(new Point3d(0, 0, 0), new Point3d(10, 0, 0));
Line3d line2 = new Line3d(new Point3d(5, -5, 0), new Point3d(5, 5, 0));

try
{
    Point3d intersection = line1.IntersectWith(line2);
    ed.WriteMessage($"\nIntersection at: {intersection}");
}
catch
{
    ed.WriteMessage("\nLines do not intersect");
}
```

### Example 4: Line-Plane Intersection
```csharp
Line3d line = new Line3d(new Point3d(0, 0, -10), new Point3d(0, 0, 10));
Plane plane = new Plane(Point3d.Origin, Vector3d.ZAxis);

try
{
    Point3d intersection = line.IntersectWith(plane);
    ed.WriteMessage($"\nLine intersects plane at: {intersection}");
}
catch
{
    ed.WriteMessage("\nLine does not intersect plane");
}
```

## Related Classes
- **LineSegment3d** - Bounded line segment
- **Point3d** - Points on line
- **Vector3d** - Direction vector
- **Plane** - For line-plane intersections

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
