# CircularArc3d Struct

## Overview
The `CircularArc3d` struct represents a circular arc in 3D space, defined by center, normal, radius, and start/end angles.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point3d` | Gets the center point of the arc |
| `Normal` | `Vector3d` | Gets the normal vector perpendicular to arc plane |
| `Radius` | `double` | Gets the radius of the arc |
| `StartAngle` | `double` | Gets the start angle (radians) |
| `EndAngle` | `double` | Gets the end angle (radians) |
| `StartPoint` | `Point3d` | Gets the start point of the arc |
| `EndPoint` | `Point3d` | Gets the end point of the arc |
| `ReferenceVector` | `Vector3d` | Gets the reference vector for angle measurement |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `CircularArc3d(Point3d, Vector3d, double)` | Creates full circle |
| `CircularArc3d(Point3d, Vector3d, Vector3d, double, double, double)` | Creates arc with all parameters |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on arc to given point |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to arc |
| `IsOn(Point3d)` | `bool` | Checks if point is on arc |

## Code Examples

### Example 1: Creating Circular Arcs
```csharp
Point3d center = new Point3d(0, 0, 0);
Vector3d normal = Vector3d.ZAxis;
double radius = 10.0;

// Full circle
CircularArc3d circle = new CircularArc3d(center, normal, radius);

ed.WriteMessage($"\nCircle radius: {circle.Radius}");
ed.WriteMessage($"\nCircle center: {circle.Center}");
```

## Related Classes
- **CircularArc2d** - 2D circular arc
- **Point3d** - Center and points on arc
- **Vector3d** - Normal vector
- **Arc** - AutoCAD arc entity

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
