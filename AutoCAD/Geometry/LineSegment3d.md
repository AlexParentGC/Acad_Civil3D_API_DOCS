# LineSegment3d Struct

## Overview
The `LineSegment3d` struct represents a bounded line segment in 3D space, defined by start and end points.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `StartPoint` | `Point3d` | Gets the start point of the segment |
| `EndPoint` | `Point3d` | Gets the end point of the segment |
| `MidPoint` | `Point3d` | Gets the midpoint of the segment |
| `Direction` | `Vector3d` | Gets the direction vector from start to end |
| `Length` | `double` | Gets the length of the segment |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `LineSegment3d(Point3d, Point3d)` | Creates segment from start and end points |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on segment to given point |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to segment |
| `IsOn(Point3d)` | `bool` | Checks if point is on segment |
| `IsParallelTo(LineSegment3d)` | `bool` | Checks if parallel to another segment |
| `IsPerpendicularTo(LineSegment3d)` | `bool` | Checks if perpendicular to another segment |

## Code Examples

### Example 1: Creating Line Segments
```csharp
Point3d start = new Point3d(0, 0, 0);
Point3d end = new Point3d(10, 10, 0);
LineSegment3d segment = new LineSegment3d(start, end);

ed.WriteMessage($"\nLength: {segment.Length:F2}");
ed.WriteMessage($"\nMidpoint: {segment.MidPoint}");
ed.WriteMessage($"\nDirection: {segment.Direction}");
```

### Example 2: Point-to-Segment Distance
```csharp
LineSegment3d segment = new LineSegment3d(
    new Point3d(0, 0, 0),
    new Point3d(10, 0, 0)
);

Point3d testPoint = new Point3d(5, 5, 0);
double distance = segment.DistanceTo(testPoint);
Point3d closestPoint = segment.GetClosestPointTo(testPoint);

ed.WriteMessage($"\nDistance to segment: {distance:F2}");
ed.WriteMessage($"\nClosest point: {closestPoint}");
```

### Example 3: Checking if Point is on Segment
```csharp
LineSegment3d segment = new LineSegment3d(
    new Point3d(0, 0, 0),
    new Point3d(10, 0, 0)
);

Point3d pt1 = new Point3d(5, 0, 0); // On segment
Point3d pt2 = new Point3d(15, 0, 0); // Beyond end
Point3d pt3 = new Point3d(5, 5, 0); // Off segment

bool isOn1 = segment.IsOn(pt1); // true
bool isOn2 = segment.IsOn(pt2); // false
bool isOn3 = segment.IsOn(pt3); // false

ed.WriteMessage($"\nPoint 1 on segment: {isOn1}");
ed.WriteMessage($"\nPoint 2 on segment: {isOn2}");
ed.WriteMessage($"\nPoint 3 on segment: {isOn3}");
```

## Related Classes
- **Line3d** - Unbounded 3D line
- **Point3d** - Start/end points
- **Vector3d** - Direction vector
- **LineSegment2d** - 2D line segment

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
