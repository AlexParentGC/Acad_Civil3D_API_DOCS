# Matrix2d Struct

## Overview
The `Matrix2d` struct represents a 3x3 transformation matrix for 2D geometric transformations. It is used for translation, rotation, scaling, and mirroring operations in the XY plane.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Inverse` | `Matrix2d` | Gets the inverse matrix |
| `IsIdentity` | `bool` | Checks if matrix is identity matrix |
| `IsUniscaledOrtho` | `bool` | Checks if matrix is orthogonal with uniform scaling |

## Static Factory Methods

| Method | Description |
|--------|-------------|
| `Identity` | Returns identity matrix (no transformation) |
| `Displacement(Vector2d)` | Creates translation matrix |
| `Rotation(double, Point2d)` | Creates rotation matrix around point |
| `Scaling(double, Point2d)` | Creates uniform scaling matrix |
| `Mirroring(Line2d)` | Creates mirror transformation across line |
| `Mirroring(Point2d)` | Creates mirror transformation across point |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `PreMultiplyBy(Matrix2d)` | `Matrix2d` | Multiplies this matrix by another |
| `PostMultiplyBy(Matrix2d)` | `Matrix2d` | Multiplies another matrix by this |
| `Transpose()` | `Matrix2d` | Returns transposed matrix |
| `Invert()` | `Matrix2d` | Returns inverted matrix |
| `IsEqualTo(Matrix2d)` | `bool` | Checks equality |

## Code Examples

### Example 1: Translation
```csharp
// Move 10 units in X, 5 in Y
Vector2d displacement = new Vector2d(10, 5);
Matrix2d translation = Matrix2d.Displacement(displacement);

Point2d pt = new Point2d(0, 0);
Point2d moved = pt.TransformBy(translation);

ed.WriteMessage($"\nOriginal: ({pt.X}, {pt.Y})");
ed.WriteMessage($"\nMoved: ({moved.X}, {moved.Y})");
```

### Example 2: Rotation
```csharp
// Rotate 45° around origin
double angle = Math.PI / 4;
Point2d center = new Point2d(0, 0);
Matrix2d rotation = Matrix2d.Rotation(angle, center);

Point2d pt = new Point2d(10, 0);
Point2d rotated = pt.TransformBy(rotation);

ed.WriteMessage($"\nRotated 45°: ({rotated.X:F2}, {rotated.Y:F2})");
```

### Example 3: Scaling
```csharp
// Scale 2x from origin
Point2d scaleBase = new Point2d(0, 0);
Matrix2d scale = Matrix2d.Scaling(2.0, scaleBase);

Point2d pt = new Point2d(5, 5);
Point2d scaled = pt.TransformBy(scale);

ed.WriteMessage($"\nScaled: ({scaled.X}, {scaled.Y})"); // (10, 10)
```

### Example 4: Combining Transformations
```csharp
// Move, rotate, scale
Vector2d move = new Vector2d(10, 5);
Matrix2d translation = Matrix2d.Displacement(move);

Matrix2d rotation = Matrix2d.Rotation(Math.PI / 4, new Point2d(0, 0));
Matrix2d scale = Matrix2d.Scaling(2.0, new Point2d(0, 0));

// Combine: scale * rotate * translate
Matrix2d combined = translation.PreMultiplyBy(rotation).PreMultiplyBy(scale);

Point2d pt = new Point2d(5, 0);
Point2d transformed = pt.TransformBy(combined);

ed.WriteMessage($"\nTransformed: ({transformed.X:F2}, {transformed.Y:F2})");
```

## Best Practices

1. **Use for 2D**: Prefer Matrix2d over Matrix3d for planar transformations
2. **Transformation Order**: Matrix multiplication order matters
3. **Combine Transformations**: Combine multiple transformations for efficiency
4. **Inverse**: Use `Inverse` to undo transformations

## Related Classes
- **Matrix3d** - 3D transformation matrix
- **Vector2d** - 2D displacement vectors
- **Point2d** - 2D points to transform
- **Line2d** - 2D line for mirroring

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Matrix2d Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Matrix2d)
