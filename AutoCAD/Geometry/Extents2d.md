# Extents2d Struct

## Overview
The `Extents2d` struct represents a 2D bounding box defined by minimum and maximum points in the XY plane.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `MinPoint` | `Point2d` | Gets/sets the minimum corner point |
| `MaxPoint` | `Point2d` | Gets/sets the maximum corner point |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `AddPoint(Point2d)` | `void` | Expands extents to include point |
| `AddExtents(Extents2d)` | `void` | Expands extents to include another extents |
| `ExpandBy(Vector2d)` | `void` | Expands extents by vector offset |
| `TransformBy(Matrix2d)` | `void` | Transforms extents by matrix |

## Code Examples

### Example 1: Creating 2D Extents
```csharp
Point2d min = new Point2d(0, 0);
Point2d max = new Point2d(100, 50);
Extents2d extents = new Extents2d(min, max);

double width = max.X - min.X;
double height = max.Y - min.Y;

ed.WriteMessage($"\nExtents: {width} x {height}");
```

### Example 2: Adding Points
```csharp
Extents2d extents = new Extents2d(new Point2d(0, 0), new Point2d(10, 10));

// Add point outside current bounds
extents.AddPoint(new Point2d(15, 5));

ed.WriteMessage($"\nNew max: {extents.MaxPoint}"); // (15, 10)
```

## Related Classes
- **Point2d** - 2D corner points
- **Vector2d** - For expanding extents
- **Extents3d** - 3D bounding box

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
