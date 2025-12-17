# Ray3d Class

## Overview
The `Ray3d` class represents a half-bounded line in 3D space, defined by a base point and a direction vector. Unlike `Line3d` (unbounded in both directions), a ray extends infinitely in only one direction from its base point.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `BasePoint` | `Point3d` | Gets the starting point of the ray |
| `Direction` | `Vector3d` | Gets the direction vector of the ray |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on ray to given point |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to ray |
| `IsOn(Point3d)` | `bool` | Checks if point is on ray |
| `IntersectWith(...)` | `Point3d` | Gets intersection with another geometric object |

## Code Examples

### Example 1: Creating Rays
```csharp
// Ray from origin pointing along positive X axis
Point3d basePoint = Point3d.Origin;
Vector3d direction = Vector3d.XAxis;

Ray3d ray = new Ray3d(basePoint, direction);

ed.WriteMessage($"\nRay created");
ed.WriteMessage($"\nBase point: {ray.BasePoint}");
ed.WriteMessage($"\nDirection: {ray.Direction}");
```

### Example 2: Ray Casting
```csharp
// Cast ray from camera position toward target
Point3d cameraPos = new Point3d(0, 0, 10);
Point3d target = new Point3d(5, 5, 0);

Vector3d direction = cameraPos.GetVectorTo(target).GetNormal();
Ray3d ray = new Ray3d(cameraPos, direction);

// Check if ray hits a point
Point3d testPoint = new Point3d(2.5, 2.5, 5);
double distance = ray.DistanceTo(testPoint);

ed.WriteMessage($"\nRay from camera to target");
ed.WriteMessage($"\nDistance to test point: {distance:F4}");
```

### Example 3: Finding Closest Point on Ray
```csharp
Ray3d ray = new Ray3d(Point3d.Origin, Vector3d.XAxis);
Point3d testPoint = new Point3d(10, 5, 0);

Point3d closestPoint = ray.GetClosestPointTo(testPoint);
double distance = ray.DistanceTo(testPoint);

ed.WriteMessage($"\nTest point: {testPoint}");
ed.WriteMessage($"\nClosest point on ray: {closestPoint}");
ed.WriteMessage($"\nDistance: {distance:F4}");
```

## Best Practices

1. **Direction**: Should be normalized (unit vector)
2. **Half-Bounded**: Extends infinitely from base point in direction only
3. **Ray Casting**: Commonly used for collision detection and picking
4. **Closest Point**: May be the base point if test point is "behind" the ray

## Related Classes
- **Ray2d** - 2D half-bounded line
- **Line3d** - Unbounded 3D line
- **LineSegment3d** - Bounded 3D line segment
- **Point3d** - Base point
- **Vector3d** - Direction vector

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Ray3d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Ray3d)
