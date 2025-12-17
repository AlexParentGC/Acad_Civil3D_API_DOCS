# Torus Class

## Overview
The `Torus` class represents a toroidal segment (donut shape) in 3D space. Tori are used in modeling pipes, rings, and organic shapes.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point3d` | Gets the center of the torus |
| `AxisOfSymmetry` | `Vector3d` | Gets the axis of rotation |
| `MajorRadius` | `double` | Gets distance from center to tube centerline |
| `MinorRadius` | `double` | Gets radius of the tube cross-section |
| `ReferenceAxis` | `Vector3d` | Gets reference axis for the torus |
| `IsApple` | `bool` | True if major radius < minor radius (apple shape) |
| `IsDoughnut` | `bool` | True if major radius > minor radius (donut shape) |
| `IsLemon` | `bool` | True if major radius = minor radius (lemon shape) |
| `IsDegenerate` | `bool` | True if either radius <= 0 |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on torus surface |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to surface |
| `IsOn(Point3d)` | `bool` | Checks if point is on surface |
| `GetAnglesInU(...)` | `void` | Gets start/end longitudes |
| `GetAnglesInV(...)` | `void` | Gets start/end latitudes |

## Code Examples

### Example 1: Creating a Torus (Donut)
```csharp
// Standard donut torus
Point3d center = Point3d.Origin;
Vector3d axis = Vector3d.ZAxis;
double majorRadius = 10.0; // Distance from center to tube center
double minorRadius = 3.0;  // Tube radius

Torus torus = new Torus(center, axis, majorRadius, minorRadius);

ed.WriteMessage($"\nTorus created");
ed.WriteMessage($"\nCenter: {torus.Center}");
ed.WriteMessage($"\nMajor radius: {torus.MajorRadius}");
ed.WriteMessage($"\nMinor radius: {torus.MinorRadius}");
ed.WriteMessage($"\nIs doughnut: {torus.IsDoughnut}"); // true
ed.WriteMessage($"\nIs apple: {torus.IsApple}"); // false
```

### Example 2: Torus Types
```csharp
Point3d center = Point3d.Origin;
Vector3d axis = Vector3d.ZAxis;

// Doughnut (major > minor)
Torus doughnut = new Torus(center, axis, 10.0, 3.0);

// Apple (major < minor)
Torus apple = new Torus(center, axis, 3.0, 10.0);

// Lemon (major = minor)
Torus lemon = new Torus(center, axis, 5.0, 5.0);

ed.WriteMessage($"\nDoughnut: IsDoughnut={doughnut.IsDoughnut}");
ed.WriteMessage($"\nApple: IsApple={apple.IsApple}");
ed.WriteMessage($"\nLemon: IsLemon={lemon.IsLemon}");
```

### Example 3: Torus Calculations
```csharp
Torus torus = new Torus(Point3d.Origin, Vector3d.ZAxis, 10.0, 3.0);

double R = torus.MajorRadius;
double r = torus.MinorRadius;

// Surface area = 4π²Rr
double surfaceArea = 4 * Math.PI * Math.PI * R * r;

// Volume = 2π²Rr²
double volume = 2 * Math.PI * Math.PI * R * r * r;

ed.WriteMessage($"\nSurface area: {surfaceArea:F2}");
ed.WriteMessage($"\nVolume: {volume:F2}");
```

### Example 4: Vertical Torus
```csharp
// Torus with axis along X (vertical ring)
Point3d center = new Point3d(0, 5, 5);
Vector3d axis = Vector3d.XAxis;
double majorRadius = 8.0;
double minorRadius = 2.0;

Torus verticalTorus = new Torus(center, axis, majorRadius, minorRadius);

ed.WriteMessage($"\nVertical torus created");
ed.WriteMessage($"\nAxis: {verticalTorus.AxisOfSymmetry}");
```

## Best Practices

1. **Major Radius**: Distance from torus center to tube centerline
2. **Minor Radius**: Radius of the tube itself
3. **Doughnut**: Most common (major > minor)
4. **Apple**: Self-intersecting (major < minor)
5. **Lemon**: Spindle torus (major = minor)
6. **Surface Area**: 4π²Rr
7. **Volume**: 2π²Rr²

## Related Classes
- **Sphere** - Spherical surface
- **Cylinder** - Cylindrical surface
- **Cone** - Conical surface
- **Point3d** - Center point
- **Vector3d** - Axis direction

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Torus Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Torus)
