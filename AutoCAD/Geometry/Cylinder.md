# Cylinder Class

## Overview
The `Cylinder` class represents a bounded right circular cylinder surface in 3D space. Cylinders are common 3D primitives used in mechanical design and modeling.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Origin` | `Point3d` | Gets the base point of the cylinder |
| `AxisOfSymmetry` | `Vector3d` | Gets the rotation axis vector |
| `Radius` | `double` | Gets the radius of the cylinder base |
| `Height` | `double` | Gets the height of the cylinder |
| `ReferenceAxis` | `Vector3d` | Gets reference vector on base circle |
| `IsOuterNormal` | `bool` | Indicates if surface normal points outward |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on cylinder surface |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to surface |
| `IsOn(Point3d)` | `bool` | Checks if point is on surface |

## Code Examples

### Example 1: Creating Cylinders
```csharp
// Cylinder with base at origin, axis along Z, radius 5, height 10
Point3d baseCenter = Point3d.Origin;
Vector3d axis = Vector3d.ZAxis;
double radius = 5.0;
double height = 10.0;

Cylinder cylinder = new Cylinder(baseCenter, axis, radius, height);

ed.WriteMessage($"\nCylinder created");
ed.WriteMessage($"\nBase: {cylinder.Origin}");
ed.WriteMessage($"\nAxis: {cylinder.AxisOfSymmetry}");
ed.WriteMessage($"\nRadius: {cylinder.Radius}");
ed.WriteMessage($"\nHeight: {cylinder.Height}");
```

### Example 2: Cylinder Calculations
```csharp
Cylinder cyl = new Cylinder(Point3d.Origin, Vector3d.ZAxis, 5.0, 10.0);

double lateralArea = 2 * Math.PI * cyl.Radius * cyl.Height;
double baseArea = Math.PI * Math.Pow(cyl.Radius, 2);
double totalSurfaceArea = lateralArea + 2 * baseArea;
double volume = baseArea * cyl.Height;

ed.WriteMessage($"\nLateral surface area: {lateralArea:F2}");
ed.WriteMessage($"\nTotal surface area: {totalSurfaceArea:F2}");
ed.WriteMessage($"\nVolume: {volume:F2}");
```

### Example 3: Horizontal Cylinder
```csharp
// Cylinder along X axis
Point3d baseCenter = new Point3d(0, 5, 5);
Vector3d axis = Vector3d.XAxis; // Horizontal
double radius = 3.0;
double height = 15.0;

Cylinder horizontalCyl = new Cylinder(baseCenter, axis, radius, height);

Point3d topCenter = new Point3d(
    baseCenter.X + height * axis.X,
    baseCenter.Y + height * axis.Y,
    baseCenter.Z + height * axis.Z
);

ed.WriteMessage($"\nHorizontal cylinder");
ed.WriteMessage($"\nBase center: {baseCenter}");
ed.WriteMessage($"\nTop center: {topCenter}");
```

## Best Practices

1. **Axis**: Axis vector should be normalized (unit vector)
2. **Height**: Measured along axis from base
3. **Orientation**: Base is at origin point, top is origin + height * axis
4. **Surface Area**: Lateral = 2πrh, Total = 2πr(r + h)
5. **Volume**: V = πr²h

## Related Classes
- **Sphere** - Spherical surface
- **Cone** - Conical surface
- **Torus** - Toroidal surface
- **Point3d** - Base center
- **Vector3d** - Axis direction

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Cylinder Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Cylinder)
