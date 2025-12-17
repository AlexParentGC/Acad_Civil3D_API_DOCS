# EllipticalArc2d Class

## Overview
The `EllipticalArc2d` class represents full ellipses and elliptical arcs in 2D space (XY plane). It provides the same functionality as `EllipticalArc3d` but optimized for planar geometry.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point2d` | Gets the center point |
| `MajorRadius` | `double` | Gets the major radius length |
| `MinorRadius` | `double` | Gets the minor radius length |
| `StartAngle` | `double` | Gets the start angle (radians) |
| `EndAngle` | `double` | Gets the end angle (radians) |
| `StartPoint` | `Point2d` | Gets the start point |
| `EndPoint` | `Point2d` | Gets the end point |
| `IsCircular` | `bool` | Checks if ellipse is a circle |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point2d)` | `Point2d` | Gets closest point on ellipse |
| `DistanceTo(Point2d)` | `double` | Gets distance from point |
| `IsOn(Point2d)` | `bool` | Checks if point is on ellipse |

## Code Examples

### Example 1: Creating 2D Ellipses
```csharp
Point2d center = new Point2d(0, 0);
double majorRadius = 10.0;
double minorRadius = 5.0;

// Full ellipse
EllipticalArc2d ellipse = new EllipticalArc2d(center, majorRadius, minorRadius, 0, 2 * Math.PI);

ed.WriteMessage($"\n2D Ellipse: Major={ellipse.MajorRadius}, Minor={ellipse.MinorRadius}");
ed.WriteMessage($"\nIs circular: {ellipse.IsCircular}");
```

### Example 2: Creating Elliptical Arcs
```csharp
Point2d center = new Point2d(10, 10);
double majorRadius = 8.0;
double minorRadius = 4.0;
double startAngle = 0;
double endAngle = Math.PI; // 180°

EllipticalArc2d arc = new EllipticalArc2d(center, majorRadius, minorRadius, startAngle, endAngle);

ed.WriteMessage($"\nArc from {startAngle * 180 / Math.PI}° to {endAngle * 180 / Math.PI}°");
ed.WriteMessage($"\nStart: ({arc.StartPoint.X:F2}, {arc.StartPoint.Y:F2})");
ed.WriteMessage($"\nEnd: ({arc.EndPoint.X:F2}, {arc.EndPoint.Y:F2})");
```

## Best Practices

1. **2D Performance**: Use EllipticalArc2d for planar work (faster than 3D)
2. **Radii Order**: Major radius >= minor radius
3. **Angles**: In radians, not degrees

## Related Classes
- **EllipticalArc3d** - 3D elliptical arc
- **CircularArc2d** - 2D circular arc
- **Point2d** - 2D points

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [EllipticalArc2d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_EllipticalArc2d)
