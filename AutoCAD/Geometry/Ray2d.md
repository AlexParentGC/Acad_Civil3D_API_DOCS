# Ray2d Class

## Overview
The `Ray2d` class represents a half-bounded line in 2D space (XY plane), defined by a base point and a direction vector.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `BasePoint` | `Point2d` | Gets the starting point of the ray |
| `Direction` | `Vector2d` | Gets the direction vector of the ray |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Gets closest point on ray |
| `DistanceTo(Point2d)` | `double` | Gets distance from point to ray |
| `IsOn(Point2d)` | `bool` | Checks if point is on ray |

## Code Examples

### Example 1: Creating 2D Rays
```csharp
Point2d basePoint = new Point2d(0, 0);
Vector2d direction = new Vector2d(1, 1).GetNormal();

Ray2d ray = new Ray2d(basePoint, direction);

ed.WriteMessage($"\n2D Ray from {ray.BasePoint} in direction {ray.Direction}");
```

### Example 2: 2D Ray Casting
```csharp
Ray2d ray = new Ray2d(new Point2d(0, 0), new Vector2d(1, 0));
Point2d testPoint = new Point2d(10, 5);

Point2d closestPoint = ray.GetClosestPointTo(testPoint);
double distance = ray.DistanceTo(testPoint);

ed.WriteMessage($"\nClosest point: ({closestPoint.X:F2}, {closestPoint.Y:F2})");
ed.WriteMessage($"\nDistance: {distance:F2}");
```

## Best Practices

1. **2D Performance**: Use Ray2d for planar ray casting (faster than 3D)
2. **Direction**: Normalize direction vector
3. **Half-Bounded**: Extends from base point in one direction only

## Related Classes
- **Ray3d** - 3D half-bounded line
- **Line2d** - Unbounded 2D line
- **LineSegment2d** - Bounded 2D line segment

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Ray2d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Ray2d)
