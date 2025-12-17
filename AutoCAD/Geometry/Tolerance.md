# Tolerance Struct

## Overview
The `Tolerance` struct defines tolerance values for geometric comparisons in AutoCAD. It provides fuzzy equality testing for points, vectors, and other geometric entities to account for floating-point precision issues.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `EqualPoint` | `double` | Tolerance for point equality comparisons |
| `EqualVector` | `double` | Tolerance for vector equality comparisons |
| `Global` | `Tolerance` (static) | Gets the global default tolerance |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `Tolerance(double, double)` | Creates tolerance with point and vector tolerances |

## Code Examples

### Example 1: Using Tolerance for Point Comparison
```csharp
Point3d pt1 = new Point3d(10.0, 20.0, 0.0);
Point3d pt2 = new Point3d(10.0001, 20.0001, 0.0);

// Without tolerance (exact comparison)
bool exactEqual = pt1 == pt2; // false

// With tolerance
Tolerance tol = new Tolerance(0.001, 0.001);
bool fuzzyEqual = pt1.IsEqualTo(pt2, tol); // true

ed.WriteMessage($"\nExact equality: {exactEqual}");
ed.WriteMessage($"\nFuzzy equality (tol=0.001): {fuzzyEqual}");
```

### Example 2: Using Global Tolerance
```csharp
// Use AutoCAD's global tolerance settings
Tolerance globalTol = Tolerance.Global;

Point3d pt1 = new Point3d(5.0, 5.0, 0.0);
Point3d pt2 = new Point3d(5.00001, 5.00001, 0.0);

bool isEqual = pt1.IsEqualTo(pt2, globalTol);

ed.WriteMessage($"\nGlobal point tolerance: {globalTol.EqualPoint}");
ed.WriteMessage($"\nGlobal vector tolerance: {globalTol.EqualVector}");
ed.WriteMessage($"\nPoints equal with global tolerance: {isEqual}");
```

### Example 3: Vector Comparison with Tolerance
```csharp
Vector3d v1 = new Vector3d(1.0, 0.0, 0.0);
Vector3d v2 = new Vector3d(1.00001, 0.00001, 0.0);

Tolerance tol = new Tolerance(0.0001, 0.0001);
bool areEqual = v1.IsEqualTo(v2, tol);

ed.WriteMessage($"\nVectors equal: {areEqual}");
```

### Example 4: Checking if Point is on Line
```csharp
Point3d lineStart = new Point3d(0, 0, 0);
Point3d lineEnd = new Point3d(10, 0, 0);
Point3d testPoint = new Point3d(5.00001, 0.00001, 0);

// Calculate distance to line
Vector3d lineVec = lineStart.GetVectorTo(lineEnd);
Vector3d pointVec = lineStart.GetVectorTo(testPoint);
Vector3d cross = lineVec.CrossProduct(pointVec);
double distance = cross.Length / lineVec.Length;

// Check with tolerance
Tolerance tol = new Tolerance(0.001, 0.001);
bool isOnLine = distance < tol.EqualPoint;

ed.WriteMessage($"\nDistance to line: {distance:F6}");
ed.WriteMessage($"\nPoint on line (within tolerance): {isOnLine}");
```

## Best Practices

1. **Use Tolerance**: Always use tolerance for floating-point geometric comparisons
2. **Global Tolerance**: Use `Tolerance.Global` for consistency with AutoCAD settings
3. **Appropriate Values**: Choose tolerance values appropriate for your application scale
4. **Point vs Vector**: Use different tolerances for points and vectors if needed

## Related Classes
- **Point3d** - Uses tolerance in `IsEqualTo()` method
- **Vector3d** - Uses tolerance in `IsEqualTo()` method
- **Point2d** - Uses tolerance for 2D comparisons
- **Vector2d** - Uses tolerance for 2D vector comparisons

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Tolerance Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Tolerance)
