# Sphere Class

## Overview
The `Sphere` class represents a spherical surface in 3D space. Spheres are fundamental 3D primitives used in modeling, collision detection, and geometric calculations.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point3d` | Gets the center point of the sphere |
| `Radius` | `double` | Gets the radius of the sphere |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on sphere surface |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to sphere surface |
| `IsOn(Point3d)` | `bool` | Checks if point is on sphere surface |
| `IsInside(Point3d)` | `bool` | Checks if point is inside sphere |

## Code Examples

### Example 1: Creating Spheres
```csharp
// Create sphere at origin with radius 10
Point3d center = Point3d.Origin;
double radius = 10.0;

Sphere sphere = new Sphere(center, radius);

ed.WriteMessage($"\nSphere created");
ed.WriteMessage($"\nCenter: {sphere.Center}");
ed.WriteMessage($"\nRadius: {sphere.Radius}");
ed.WriteMessage($"\nSurface Area: {4 * Math.PI * radius * radius:F2}");
ed.WriteMessage($"\nVolume: {(4.0 / 3.0) * Math.PI * Math.Pow(radius, 3):F2}");
```

### Example 2: Point-Sphere Relationship
```csharp
Sphere sphere = new Sphere(new Point3d(0, 0, 0), 10.0);

Point3d insidePoint = new Point3d(5, 0, 0);
Point3d onSurface = new Point3d(10, 0, 0);
Point3d outsidePoint = new Point3d(15, 0, 0);

ed.WriteMessage($"\nInside point is inside: {sphere.IsInside(insidePoint)}");
ed.WriteMessage($"\nSurface point is on surface: {sphere.IsOn(onSurface)}");
ed.WriteMessage($"\nOutside point distance: {sphere.DistanceTo(outsidePoint):F2}");
```

### Example 3: Finding Closest Point on Sphere
```csharp
Sphere sphere = new Sphere(Point3d.Origin, 10.0);
Point3d testPoint = new Point3d(20, 20, 20);

Point3d closestPoint = sphere.GetClosestPointTo(testPoint);
double distance = sphere.DistanceTo(testPoint);

ed.WriteMessage($"\nTest point: {testPoint}");
ed.WriteMessage($"\nClosest point on sphere: {closestPoint}");
ed.WriteMessage($"\nDistance to surface: {distance:F4}");
```

## Best Practices

1. **Radius**: Must be positive
2. **Surface vs Volume**: Use `IsOn()` for surface, `IsInside()` for volume
3. **Distance**: Distance is to surface, not center
4. **Calculations**: Surface area = 4πr², Volume = (4/3)πr³

## Related Classes
- **Cylinder** - Cylindrical surface
- **Cone** - Conical surface
- **Torus** - Toroidal surface
- **Point3d** - Center and points on sphere

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Sphere Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Sphere)
