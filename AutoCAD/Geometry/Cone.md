# Cone Class

## Overview
The `Cone` class represents a bounded right circular cone in 3D space. Cones are fundamental 3D primitives used in modeling tapered shapes and geometric calculations.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `BaseCenter` | `Point3d` | Gets the center of the cone base |
| `Apex` | `Point3d` | Gets the apex (tip) of the cone |
| `AxisOfSymmetry` | `Vector3d` | Gets the axis from base to apex |
| `BaseRadius` | `double` | Gets the radius of the cone base |
| `Height` | `double` | Gets the height of the cone |
| `HalfAngle` | `double` | Gets the half-angle of the cone (radians) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on cone surface |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to surface |
| `IsOn(Point3d)` | `bool` | Checks if point is on surface |

## Code Examples

### Example 1: Creating Cones
```csharp
// Cone with base at origin, apex at (0, 0, 10), base radius 5
Point3d baseCenter = Point3d.Origin;
Point3d apex = new Point3d(0, 0, 10);
double baseRadius = 5.0;

Cone cone = new Cone(baseCenter, apex, baseRadius);

double height = baseCenter.DistanceTo(apex);
double halfAngle = Math.Atan(baseRadius / height);

ed.WriteMessage($"\nCone created");
ed.WriteMessage($"\nBase center: {cone.BaseCenter}");
ed.WriteMessage($"\nApex: {cone.Apex}");
ed.WriteMessage($"\nBase radius: {cone.BaseRadius}");
ed.WriteMessage($"\nHeight: {height:F2}");
ed.WriteMessage($"\nHalf angle: {halfAngle * 180 / Math.PI:F2}°");
```

### Example 2: Cone Calculations
```csharp
Cone cone = new Cone(Point3d.Origin, new Point3d(0, 0, 10), 5.0);

double height = 10.0;
double radius = 5.0;
double slantHeight = Math.Sqrt(height * height + radius * radius);

double baseArea = Math.PI * radius * radius;
double lateralArea = Math.PI * radius * slantHeight;
double totalSurfaceArea = baseArea + lateralArea;
double volume = (1.0 / 3.0) * baseArea * height;

ed.WriteMessage($"\nSlant height: {slantHeight:F2}");
ed.WriteMessage($"\nBase area: {baseArea:F2}");
ed.WriteMessage($"\nLateral surface area: {lateralArea:F2}");
ed.WriteMessage($"\nTotal surface area: {totalSurfaceArea:F2}");
ed.WriteMessage($"\nVolume: {volume:F2}");
```

### Example 3: Frustum of Cone
```csharp
// Create cone then calculate frustum (truncated cone)
Point3d baseCenter = Point3d.Origin;
Point3d apex = new Point3d(0, 0, 15);
double baseRadius = 6.0;

Cone fullCone = new Cone(baseCenter, apex, baseRadius);

// Frustum: cut at height 10 (top radius will be 2)
double cutHeight = 10.0;
double fullHeight = 15.0;
double topRadius = baseRadius * (fullHeight - cutHeight) / fullHeight;

ed.WriteMessage($"\nFrustum parameters:");
ed.WriteMessage($"\nBase radius: {baseRadius}");
ed.WriteMessage($"\nTop radius: {topRadius:F2}");
ed.WriteMessage($"\nHeight: {cutHeight}");

// Frustum volume = (1/3)πh(r1² + r1*r2 + r2²)
double frustumVolume = (1.0 / 3.0) * Math.PI * cutHeight * 
    (baseRadius * baseRadius + baseRadius * topRadius + topRadius * topRadius);

ed.WriteMessage($"\nFrustum volume: {frustumVolume:F2}");
```

## Best Practices

1. **Apex Position**: Apex must not be at base center
2. **Base Radius**: Must be positive
3. **Half Angle**: tan(halfAngle) = baseRadius / height
4. **Surface Area**: Lateral = πrs (s = slant height), Total = πr(r + s)
5. **Volume**: V = (1/3)πr²h
6. **Frustum**: For truncated cone, use proportional calculations

## Related Classes
- **Cylinder** - Cylindrical surface (cone with 0° angle)
- **Sphere** - Spherical surface
- **Point3d** - Base center and apex
- **Vector3d** - Axis direction

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Cone Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Cone)
