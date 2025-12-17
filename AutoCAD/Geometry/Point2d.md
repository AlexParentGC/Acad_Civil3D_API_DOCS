# Point2d Struct

## Overview
The `Point2d` struct represents a point in 2D space with X and Y coordinates. It is used for planar geometry operations in the XY plane.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `X` | `double` | Gets the X coordinate |
| `Y` | `double` | Gets the Y coordinate |
| `Origin` | `Point2d` (static) | Gets the origin point (0, 0) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Add(Vector2d)` | `Point2d` | Adds a vector to the point |
| `Subtract(Vector2d)` | `Point2d` | Subtracts a vector from the point |
| `GetVectorTo(Point2d)` | `Vector2d` | Gets the vector from this point to another |
| `DistanceTo(Point2d)` | `double` | Calculates distance to another point |
| `TransformBy(Matrix2d)` | `Point2d` | Transforms the point by a matrix |
| `RotateBy(double, Point2d)` | `Point2d` | Rotates the point around another point |
| `Mirror(Line2d)` | `Point2d` | Mirrors the point across a line |
| `ToArray()` | `double[]` | Converts to array [X, Y] |
| `IsEqualTo(Point2d)` | `bool` | Checks equality |

## Code Examples

### Example 1: Creating Points
```csharp
// Create points
Point2d origin = Point2d.Origin; // (0, 0)
Point2d pt1 = new Point2d(10, 20);
Point2d pt2 = new Point2d(5.5, 7.3);

// From array
double[] coords = { 15, 25 };
Point2d pt3 = new Point2d(coords);

ed.WriteMessage($"\nPoint 1: ({pt1.X}, {pt1.Y})");
ed.WriteMessage($"\nPoint 2: ({pt2.X}, {pt2.Y})");
```

### Example 2: Point Arithmetic
```csharp
Point2d pt = new Point2d(10, 10);
Vector2d vec = new Vector2d(5, 3);

// Add vector
Point2d newPt = pt.Add(vec); // (15, 13)

// Subtract vector
Point2d movedPt = pt.Subtract(vec); // (5, 7)

// Get vector between points
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 10);
Vector2d direction = pt1.GetVectorTo(pt2);

ed.WriteMessage($"\nDirection: ({direction.X}, {direction.Y})");
```

### Example 3: Distance Calculations
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 0);
Point2d pt3 = new Point2d(0, 10);

double dist12 = pt1.DistanceTo(pt2); // 10.0
double dist13 = pt1.DistanceTo(pt3); // 10.0
double dist23 = pt2.DistanceTo(pt3); // 14.14

ed.WriteMessage($"\nDistance pt1 to pt2: {dist12:F2}");
ed.WriteMessage($"\nDistance pt2 to pt3: {dist23:F2}");
```

### Example 4: Transformations
```csharp
Point2d pt = new Point2d(10, 0);

// Rotate 90°
Point2d rotated = pt.RotateBy(Math.PI / 2, Point2d.Origin);

// Transform by matrix
Matrix2d translation = Matrix2d.Displacement(new Vector2d(5, 10));
Point2d translated = pt.TransformBy(translation);

ed.WriteMessage($"\nRotated: ({rotated.X:F2}, {rotated.Y:F2})");
ed.WriteMessage($"\nTranslated: ({translated.X}, {translated.Y})");
```

### Example 5: Converting to 3D
```csharp
Point2d pt2d = new Point2d(10, 20);

// Convert to 3D (Z = 0)
Point3d pt3d = new Point3d(pt2d.X, pt2d.Y, 0);

ed.WriteMessage($"\n2D: ({pt2d.X}, {pt2d.Y})");
ed.WriteMessage($"\n3D: ({pt3d.X}, {pt3d.Y}, {pt3d.Z})");

// Convert from 3D to 2D
Point3d pt3dInput = new Point3d(5, 10, 15);
Point2d pt2dResult = new Point2d(pt3dInput.X, pt3dInput.Y);

ed.WriteMessage($"\n3D to 2D: ({pt2dResult.X}, {pt2dResult.Y})");
```

## Common Patterns

### Midpoint Calculation
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(20, 10);

Point2d midpoint = new Point2d(
    (pt1.X + pt2.X) / 2,
    (pt1.Y + pt2.Y) / 2
);

// Alternative using vectors
Vector2d halfVec = pt1.GetVectorTo(pt2) * 0.5;
Point2d midpoint2 = pt1.Add(halfVec);
```

### Interpolation
```csharp
Point2d start = new Point2d(0, 0);
Point2d end = new Point2d(10, 10);

// Get point 30% along the line
double t = 0.3;
Vector2d vec = start.GetVectorTo(end) * t;
Point2d interpolated = start.Add(vec);
```

## Best Practices

1. **Use for 2D**: Prefer Point2d over Point3d for planar operations
2. **Origin**: Use `Point2d.Origin` instead of `new Point2d(0, 0)`
3. **Immutability**: Point2d is a struct; operations return new points
4. **Tolerance**: Use tolerance-based equality for comparisons
5. **Conversion**: Easy conversion to/from Point3d when needed

## Related Classes
- **Point3d** - 3D point with X, Y, Z coordinates
- **Vector2d** - 2D direction and magnitude
- **Matrix2d** - 2D transformation matrix
- **Line2d** - 2D line using two points

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Point2d Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Point2d)
