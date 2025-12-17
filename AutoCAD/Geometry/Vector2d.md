# Vector2d Struct

## Overview
The `Vector2d` struct represents a direction and magnitude in 2D space (XY plane). It is used for planar geometric calculations, 2D transformations, and operations that don't require a Z component.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `X` | `double` | Gets the X component |
| `Y` | `double` | Gets the Y component |
| `Length` | `double` | Gets the length (magnitude) of the vector |
| `LengthSqrd` | `double` | Gets the squared length (faster than Length) |
| `Angle` | `double` | Gets the angle from X axis (radians) |
| `XAxis` | `Vector2d` (static) | Gets the unit vector (1, 0) |
| `YAxis` | `Vector2d` (static) | Gets the unit vector (0, 1) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetNormal()` | `Vector2d` | Returns normalized unit vector |
| `DotProduct(Vector2d)` | `double` | Calculates dot product with another vector |
| `GetAngleTo(Vector2d)` | `double` | Gets angle to another vector (radians) |
| `TransformBy(Matrix2d)` | `Vector2d` | Transforms vector by 2D matrix |
| `RotateBy(double)` | `Vector2d` | Rotates vector by angle |
| `Negate()` | `Vector2d` | Returns negated vector |
| `MultiplyBy(double)` | `Vector2d` | Scales vector by scalar |
| `IsParallelTo(Vector2d)` | `bool` | Checks if parallel to another vector |
| `IsPerpendicularTo(Vector2d)` | `bool` | Checks if perpendicular to another vector |
| `IsCodirectionalTo(Vector2d)` | `bool` | Checks if pointing in same direction |
| `IsEqualTo(Vector2d)` | `bool` | Checks equality with default tolerance |
| `IsEqualTo(Vector2d, Tolerance)` | `bool` | Checks equality with custom tolerance |

## Operator Overloads

| Operator | Description |
|----------|-------------|
| `+` | Vector addition |
| `-` | Vector subtraction |
| `*` | Scalar multiplication |
| `/` | Scalar division |
| `==` | Equality comparison |
| `!=` | Inequality comparison |

## Code Examples

### Example 1: Creating and Using 2D Vectors
```csharp
// Create 2D vectors
Vector2d vec1 = new Vector2d(3, 4);
Vector2d vec2 = new Vector2d(1, 0);

// Get properties
double length = vec1.Length; // 5.0
double angle = vec1.Angle; // Angle from X axis in radians
double angleDegrees = angle * (180.0 / Math.PI);

// Normalize
Vector2d unitVec = vec1.GetNormal(); // (0.6, 0.8)

// Use predefined axes
Vector2d xAxis = Vector2d.XAxis; // (1, 0)
Vector2d yAxis = Vector2d.YAxis; // (0, 1)

ed.WriteMessage($"\nVector: ({vec1.X}, {vec1.Y})");
ed.WriteMessage($"\nLength: {length:F2}");
ed.WriteMessage($"\nAngle: {angleDegrees:F2}°");
ed.WriteMessage($"\nUnit vector: ({unitVec.X:F2}, {unitVec.Y:F2})");
```

### Example 2: 2D Vector Arithmetic
```csharp
Vector2d v1 = new Vector2d(5, 3);
Vector2d v2 = new Vector2d(2, 1);

// Addition
Vector2d sum = v1 + v2; // (7, 4)

// Subtraction
Vector2d diff = v1 - v2; // (3, 2)

// Scalar operations
Vector2d scaled = v1 * 2.0; // (10, 6)
Vector2d divided = v1 / 2.0; // (2.5, 1.5)

// Negation
Vector2d negated = v1.Negate(); // (-5, -3)

ed.WriteMessage($"\nSum: ({sum.X}, {sum.Y})");
ed.WriteMessage($"\nDifference: ({diff.X}, {diff.Y})");
ed.WriteMessage($"\nScaled: ({scaled.X}, {scaled.Y})");
```

### Example 3: Angle Calculations
```csharp
Vector2d v1 = new Vector2d(1, 0); // X axis
Vector2d v2 = new Vector2d(1, 1); // 45° from X axis
Vector2d v3 = new Vector2d(0, 1); // Y axis

// Get angles
double angle12 = v1.GetAngleTo(v2); // π/4 (45°)
double angle13 = v1.GetAngleTo(v3); // π/2 (90°)

// Convert to degrees
double degrees12 = angle12 * (180.0 / Math.PI);
double degrees13 = angle13 * (180.0 / Math.PI);

ed.WriteMessage($"\nAngle v1 to v2: {degrees12:F2}°");
ed.WriteMessage($"\nAngle v1 to v3: {degrees13:F2}°");

// Get angle property (from X axis)
double v2Angle = v2.Angle * (180.0 / Math.PI);
ed.WriteMessage($"\nv2 angle from X axis: {v2Angle:F2}°");
```

### Example 4: Rotation
```csharp
Vector2d vec = new Vector2d(10, 0);

// Rotate 45° counterclockwise
double angle = Math.PI / 4;
Vector2d rotated = vec.RotateBy(angle);

// Rotate 90° to get perpendicular
Vector2d perpendicular = vec.RotateBy(Math.PI / 2);

ed.WriteMessage($"\nOriginal: ({vec.X:F2}, {vec.Y:F2})");
ed.WriteMessage($"\nRotated 45°: ({rotated.X:F2}, {rotated.Y:F2})");
ed.WriteMessage($"\nPerpendicular: ({perpendicular.X:F2}, {perpendicular.Y:F2})");

// Create circular pattern
for (int i = 0; i < 8; i++)
{
    double ang = (i * 2 * Math.PI) / 8;
    Vector2d rotVec = vec.RotateBy(ang);
    ed.WriteMessage($"\nPoint {i}: ({rotVec.X:F2}, {rotVec.Y:F2})");
}
```

### Example 5: Dot Product and Perpendicularity
```csharp
Vector2d v1 = new Vector2d(3, 4);
Vector2d v2 = new Vector2d(-4, 3); // Perpendicular to v1

// Dot product
double dot = v1.DotProduct(v2); // Should be 0 for perpendicular

// Check relationships
bool isPerp = v1.IsPerpendicularTo(v2);
bool isParallel = v1.IsParallelTo(v2);

ed.WriteMessage($"\nDot product: {dot:F6}");
ed.WriteMessage($"\nPerpendicular: {isPerp}");
ed.WriteMessage($"\nParallel: {isParallel}");

// Practical use: check if vectors are perpendicular
Vector2d a = new Vector2d(1, 2);
Vector2d b = new Vector2d(-2, 1);
double dotAB = a.DotProduct(b);
ed.WriteMessage($"\nVectors perpendicular: {Math.Abs(dotAB) < 0.001}");
```

### Example 6: 2D Projection
```csharp
// Project vector A onto vector B
Vector2d vecA = new Vector2d(5, 3);
Vector2d vecB = new Vector2d(1, 0); // X axis

// Projection formula: proj_B(A) = (A · B / |B|²) * B
double dotProduct = vecA.DotProduct(vecB);
double lengthSqrd = vecB.LengthSqrd;
Vector2d projection = vecB * (dotProduct / lengthSqrd);

// Perpendicular component
Vector2d perpComponent = vecA - projection;

ed.WriteMessage($"\nVector A: ({vecA.X}, {vecA.Y})");
ed.WriteMessage($"\nProjection onto X axis: ({projection.X:F2}, {projection.Y:F2})");
ed.WriteMessage($"\nPerpendicular component: ({perpComponent.X:F2}, {perpComponent.Y:F2})");
```

### Example 7: Converting Between 2D and 3D
```csharp
// 2D to 3D
Vector2d vec2d = new Vector2d(5, 3);
Vector3d vec3d = new Vector3d(vec2d.X, vec2d.Y, 0);

ed.WriteMessage($"\n2D Vector: ({vec2d.X}, {vec2d.Y})");
ed.WriteMessage($"\n3D Vector: ({vec3d.X}, {vec3d.Y}, {vec3d.Z})");

// 3D to 2D (ignoring Z)
Vector3d vec3dInput = new Vector3d(10, 20, 30);
Vector2d vec2dResult = new Vector2d(vec3dInput.X, vec3dInput.Y);

ed.WriteMessage($"\n3D Input: ({vec3dInput.X}, {vec3dInput.Y}, {vec3dInput.Z})");
ed.WriteMessage($"\n2D Result: ({vec2dResult.X}, {vec2dResult.Y})");
```

### Example 8: Creating Offset in 2D
```csharp
// Create offset perpendicular to direction
Point2d start = new Point2d(0, 0);
Point2d end = new Point2d(10, 0);
double offsetDist = 5.0;

// Get direction vector
Vector2d direction = new Vector2d(end.X - start.X, end.Y - start.Y);

// Get perpendicular (rotate 90°)
Vector2d perpendicular = direction.RotateBy(Math.PI / 2);
Vector2d offsetVector = perpendicular.GetNormal() * offsetDist;

// Calculate offset points
Point2d offsetStart = new Point2d(start.X + offsetVector.X, start.Y + offsetVector.Y);
Point2d offsetEnd = new Point2d(end.X + offsetVector.X, end.Y + offsetVector.Y);

ed.WriteMessage($"\nOriginal: ({start.X}, {start.Y}) to ({end.X}, {end.Y})");
ed.WriteMessage($"\nOffset: ({offsetStart.X:F2}, {offsetStart.Y:F2}) to ({offsetEnd.X:F2}, {offsetEnd.Y:F2})");
```

## Common Patterns

### Getting Direction Between 2D Points
```csharp
Point2d pt1 = new Point2d(0, 0);
Point2d pt2 = new Point2d(10, 10);

Vector2d direction = new Vector2d(pt2.X - pt1.X, pt2.Y - pt1.Y);
Vector2d unitDirection = direction.GetNormal();
double distance = direction.Length;
```

### Checking Point Side (Left/Right of Line)
```csharp
Point2d lineStart = new Point2d(0, 0);
Point2d lineEnd = new Point2d(10, 0);
Point2d testPoint = new Point2d(5, 5);

Vector2d lineVec = new Vector2d(lineEnd.X - lineStart.X, lineEnd.Y - lineStart.Y);
Vector2d pointVec = new Vector2d(testPoint.X - lineStart.X, testPoint.Y - lineStart.Y);

// Cross product in 2D (Z component)
double cross = lineVec.X * pointVec.Y - lineVec.Y * pointVec.X;

if (cross > 0)
    ed.WriteMessage("\nPoint is on the left");
else if (cross < 0)
    ed.WriteMessage("\nPoint is on the right");
else
    ed.WriteMessage("\nPoint is on the line");
```

### Creating Unit Vector from Angle
```csharp
double angleDegrees = 45;
double angleRadians = angleDegrees * (Math.PI / 180.0);

Vector2d unitVec = new Vector2d(Math.Cos(angleRadians), Math.Sin(angleRadians));

ed.WriteMessage($"\nUnit vector at {angleDegrees}°: ({unitVec.X:F4}, {unitVec.Y:F4})");
```

## Best Practices

1. **Use for 2D Operations**: Prefer Vector2d over Vector3d when working in XY plane
2. **Performance**: Vector2d operations are faster than Vector3d for planar geometry
3. **Angle Property**: Use `Angle` property to get vector direction from X axis
4. **Normalization**: Always normalize when you need direction only
5. **Tolerance**: Use tolerance-based equality for floating-point comparisons
6. **Immutability**: Vector2d is a struct; operations return new vectors
7. **Conversion**: Easy conversion to/from Vector3d when needed

## Related Classes
- **Vector3d** - 3D vector with X, Y, Z components
- **Point2d** - 2D point in space
- **Matrix2d** - 2D transformation matrix
- **Line2d** - Unbounded 2D line
- **LineSegment2d** - Bounded 2D line segment
- **Tolerance** - Tolerance for comparisons

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Vector2d Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Vector2d)
- [Geometry Namespace](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
