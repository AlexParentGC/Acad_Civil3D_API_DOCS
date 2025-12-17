# EllipticalArc3d Class

## Overview
The `EllipticalArc3d` class represents full ellipses and elliptical arcs in 3D space. Ellipses are fundamental geometric shapes in CAD, commonly used for holes, slots, and organic curves.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point3d` | Gets the center point of the ellipse |
| `MajorAxis` | `Vector3d` | Gets the major axis vector (unit vector) |
| `MinorAxis` | `Vector3d` | Gets the minor axis vector (unit vector) |
| `MajorRadius` | `double` | Gets the length of the major radius |
| `MinorRadius` | `double` | Gets the length of the minor radius |
| `Normal` | `Vector3d` | Gets the normal vector to the ellipse plane |
| `StartAngle` | `double` | Gets the start angle (radians) |
| `EndAngle` | `double` | Gets the end angle (radians) |
| `StartPoint` | `Point3d` | Gets the start point of the arc |
| `EndPoint` | `Point3d` | Gets the end point of the arc |
| `IsCircular` | `bool` | Checks if the ellipse is actually a circle |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on ellipse |
| `DistanceTo(Point3d)` | `double` | Gets distance from point to ellipse |
| `IsOn(Point3d)` | `bool` | Checks if point is on ellipse |
| `GetPlane()` | `Plane` | Gets the infinite plane containing the ellipse |
| `SetAngles(double, double)` | `void` | Changes start and end angles |
| `SetAxes(Vector3d, Vector3d)` | `void` | Resets major and minor axes |
| `IntersectWith(...)` | `Point3d[]` | Calculates intersections |

## Code Examples

### Example 1: Creating Ellipses
```csharp
// Full ellipse
Point3d center = new Point3d(0, 0, 0);
Vector3d majorAxis = new Vector3d(10, 0, 0);
Vector3d minorAxis = new Vector3d(0, 5, 0);
Vector3d normal = Vector3d.ZAxis;

EllipticalArc3d fullEllipse = new EllipticalArc3d(center, normal, majorAxis, minorAxis, 0, 2 * Math.PI);

ed.WriteMessage($"\nFull ellipse created");
ed.WriteMessage($"\nMajor radius: {fullEllipse.MajorRadius}");
ed.WriteMessage($"\nMinor radius: {fullEllipse.MinorRadius}");
ed.WriteMessage($"\nIs circular: {fullEllipse.IsCircular}");
```

### Example 2: Creating Elliptical Arcs
```csharp
Point3d center = new Point3d(10, 10, 0);
Vector3d majorAxis = new Vector3d(8, 0, 0);
Vector3d minorAxis = new Vector3d(0, 4, 0);
Vector3d normal = Vector3d.ZAxis;

// Arc from 0° to 180°
double startAngle = 0;
double endAngle = Math.PI;

EllipticalArc3d arc = new EllipticalArc3d(center, normal, majorAxis, minorAxis, startAngle, endAngle);

ed.WriteMessage($"\nElliptical arc created");
ed.WriteMessage($"\nStart point: {arc.StartPoint}");
ed.WriteMessage($"\nEnd point: {arc.EndPoint}");
ed.WriteMessage($"\nArc angle: {(endAngle - startAngle) * 180 / Math.PI}°");
```

### Example 3: Checking if Ellipse is a Circle
```csharp
EllipticalArc3d ellipse1 = new EllipticalArc3d(
    Point3d.Origin,
    Vector3d.ZAxis,
    new Vector3d(5, 0, 0),
    new Vector3d(0, 5, 0),
    0, 2 * Math.PI
);

EllipticalArc3d ellipse2 = new EllipticalArc3d(
    Point3d.Origin,
    Vector3d.ZAxis,
    new Vector3d(10, 0, 0),
    new Vector3d(0, 5, 0),
    0, 2 * Math.PI
);

ed.WriteMessage($"\nEllipse 1 is circular: {ellipse1.IsCircular}"); // true (5 = 5)
ed.WriteMessage($"\nEllipse 2 is circular: {ellipse2.IsCircular}"); // false (10 ≠ 5)
```

### Example 4: Modifying Ellipse Angles
```csharp
EllipticalArc3d arc = /* existing elliptical arc */;

// Change to quarter arc (0° to 90°)
arc.SetAngles(0, Math.PI / 2);

ed.WriteMessage($"\nNew start angle: {arc.StartAngle * 180 / Math.PI}°");
ed.WriteMessage($"\nNew end angle: {arc.EndAngle * 180 / Math.PI}°");
ed.WriteMessage($"\nNew start point: {arc.StartPoint}");
ed.WriteMessage($"\nNew end point: {arc.EndPoint}");
```

### Example 5: Finding Closest Point on Ellipse
```csharp
EllipticalArc3d ellipse = new EllipticalArc3d(
    new Point3d(0, 0, 0),
    Vector3d.ZAxis,
    new Vector3d(10, 0, 0),
    new Vector3d(0, 5, 0),
    0, 2 * Math.PI
);

Point3d testPoint = new Point3d(15, 3, 0);
Point3d closestPoint = ellipse.GetClosestPointTo(testPoint);
double distance = ellipse.DistanceTo(testPoint);

ed.WriteMessage($"\nTest point: {testPoint}");
ed.WriteMessage($"\nClosest point on ellipse: {closestPoint}");
ed.WriteMessage($"\nDistance: {distance:F4}");
```

### Example 6: Creating Ellipse in Different Plane
```csharp
// Ellipse in YZ plane (vertical)
Point3d center = new Point3d(0, 10, 5);
Vector3d normal = Vector3d.XAxis; // Normal points along X
Vector3d majorAxis = new Vector3d(0, 8, 0); // Along Y
Vector3d minorAxis = new Vector3d(0, 0, 4); // Along Z

EllipticalArc3d verticalEllipse = new EllipticalArc3d(
    center, normal, majorAxis, minorAxis, 0, 2 * Math.PI
);

Plane ellipsePlane = verticalEllipse.GetPlane();

ed.WriteMessage($"\nVertical ellipse created");
ed.WriteMessage($"\nPlane normal: {ellipsePlane.Normal}");
ed.WriteMessage($"\nPlane point: {ellipsePlane.PointOnPlane}");
```

## Common Patterns

### Creating Ellipse from Radii
```csharp
Point3d center = new Point3d(0, 0, 0);
double majorRadius = 10.0;
double minorRadius = 5.0;
Vector3d normal = Vector3d.ZAxis;

// Major axis along X, minor along Y
Vector3d majorAxis = new Vector3d(majorRadius, 0, 0);
Vector3d minorAxis = new Vector3d(0, minorRadius, 0);

EllipticalArc3d ellipse = new EllipticalArc3d(
    center, normal, majorAxis, minorAxis, 0, 2 * Math.PI
);
```

### Converting Ellipse to Circle
```csharp
EllipticalArc3d ellipse = /* existing ellipse */;

if (ellipse.IsCircular)
{
    double radius = ellipse.MajorRadius; // or MinorRadius, they're equal
    CircularArc3d circle = new CircularArc3d(
        ellipse.Center,
        ellipse.Normal,
        radius
    );
}
```

## Best Practices

1. **Major vs Minor**: Major radius must be >= minor radius
2. **Circular Check**: Use `IsCircular` property instead of comparing radii
3. **Angles**: Angles are in radians, not degrees
4. **Normal Vector**: Ensure normal is normalized (unit vector)
5. **Full Ellipse**: Use startAngle = 0, endAngle = 2π for full ellipse
6. **Plane**: Ellipse lies in plane defined by center and normal

## Related Classes
- **EllipticalArc2d** - 2D elliptical arc
- **CircularArc3d** - Circular arc (special case)
- **Point3d** - Center and points on ellipse
- **Vector3d** - Axes and normal
- **Plane** - Plane containing ellipse

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [EllipticalArc3d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_EllipticalArc3d)
