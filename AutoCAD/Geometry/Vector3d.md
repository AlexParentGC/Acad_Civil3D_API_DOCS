# Vector3d Struct

## Overview
The `Vector3d` struct represents a direction and magnitude in 3D space. It is a fundamental geometry class in the AutoCAD .NET API, used extensively for directional calculations, transformations, normal vectors, and geometric operations.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `X` | `double` | Gets the X component |
| `Y` | `double` | Gets the Y component |
| `Z` | `double` | Gets the Z component |
| `Length` | `double` | Gets the length (magnitude) of the vector |
| `LengthSqrd` | `double` | Gets the squared length (faster than Length) |
| `XAxis` | `Vector3d` (static) | Gets the unit vector (1, 0, 0) |
| `YAxis` | `Vector3d` (static) | Gets the unit vector (0, 1, 0) |
| `ZAxis` | `Vector3d` (static) | Gets the unit vector (0, 0, 1) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetNormal()` | `Vector3d` | Returns normalized unit vector |
| `DotProduct(Vector3d)` | `double` | Calculates dot product with another vector |
| `CrossProduct(Vector3d)` | `Vector3d` | Calculates cross product (perpendicular vector) |
| `GetAngleTo(Vector3d)` | `double` | Gets angle to another vector (radians) |
| `GetAngleTo(Vector3d, Vector3d)` | `double` | Gets signed angle around reference vector |
| `TransformBy(Matrix3d)` | `Vector3d` | Transforms vector by matrix |
| `RotateBy(double, Vector3d)` | `Vector3d` | Rotates vector around axis |
| `Negate()` | `Vector3d` | Returns negated vector |
| `MultiplyBy(double)` | `Vector3d` | Scales vector by scalar |
| `IsParallelTo(Vector3d)` | `bool` | Checks if parallel to another vector |
| `IsPerpendicularTo(Vector3d)` | `bool` | Checks if perpendicular to another vector |
| `IsCodirectionalTo(Vector3d)` | `bool` | Checks if pointing in same direction |
| `IsEqualTo(Vector3d)` | `bool` | Checks equality with default tolerance |
| `IsEqualTo(Vector3d, Tolerance)` | `bool` | Checks equality with custom tolerance |

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

### Example 1: Creating and Normalizing Vectors
```csharp
// Create vectors
Vector3d vec1 = new Vector3d(3, 4, 0);
Vector3d vec2 = new Vector3d(10, 0, 0);

// Get vector properties
double length = vec1.Length; // 5.0
double lengthSqrd = vec1.LengthSqrd; // 25.0 (faster calculation)

// Normalize to unit vector
Vector3d unitVec = vec1.GetNormal(); // (0.6, 0.8, 0)

// Use predefined axis vectors
Vector3d xAxis = Vector3d.XAxis; // (1, 0, 0)
Vector3d yAxis = Vector3d.YAxis; // (0, 1, 0)
Vector3d zAxis = Vector3d.ZAxis; // (0, 0, 1)

ed.WriteMessage($"\nVector: ({vec1.X}, {vec1.Y}, {vec1.Z})");
ed.WriteMessage($"\nLength: {length:F2}");
ed.WriteMessage($"\nUnit vector: ({unitVec.X:F2}, {unitVec.Y:F2}, {unitVec.Z:F2})");
```

### Example 2: Vector Arithmetic
```csharp
Vector3d v1 = new Vector3d(1, 2, 3);
Vector3d v2 = new Vector3d(4, 5, 6);

// Addition
Vector3d sum = v1 + v2; // (5, 7, 9)

// Subtraction
Vector3d diff = v2 - v1; // (3, 3, 3)

// Scalar multiplication
Vector3d scaled = v1 * 2.5; // (2.5, 5, 7.5)

// Scalar division
Vector3d divided = v1 / 2.0; // (0.5, 1, 1.5)

// Negation
Vector3d negated = v1.Negate(); // (-1, -2, -3)

ed.WriteMessage($"\nSum: {sum}");
ed.WriteMessage($"\nDifference: {diff}");
ed.WriteMessage($"\nScaled: {scaled}");
```

### Example 3: Dot Product and Cross Product
```csharp
Vector3d v1 = new Vector3d(1, 0, 0);
Vector3d v2 = new Vector3d(0, 1, 0);

// Dot product (scalar result)
double dot = v1.DotProduct(v2); // 0 (perpendicular vectors)

// Cross product (vector result - perpendicular to both)
Vector3d cross = v1.CrossProduct(v2); // (0, 0, 1) - Z axis

// Practical example: check if vectors are perpendicular
Vector3d a = new Vector3d(3, 4, 0);
Vector3d b = new Vector3d(-4, 3, 0);
double dotAB = a.DotProduct(b); // 0 (perpendicular)

ed.WriteMessage($"\nDot product: {dot}");
ed.WriteMessage($"\nCross product: ({cross.X}, {cross.Y}, {cross.Z})");
ed.WriteMessage($"\nVectors perpendicular: {Math.Abs(dotAB) < 0.001}");
```

### Example 4: Angle Calculations
```csharp
Vector3d v1 = new Vector3d(1, 0, 0); // X axis
Vector3d v2 = new Vector3d(1, 1, 0); // 45° from X axis

// Get angle between vectors (always positive, 0 to π)
double angle = v1.GetAngleTo(v2);
double degrees = angle * (180.0 / Math.PI); // Convert to degrees

// Get signed angle (with reference vector)
Vector3d refVec = Vector3d.ZAxis;
double signedAngle = v1.GetAngleTo(v2, refVec);

ed.WriteMessage($"\nAngle: {angle:F4} radians ({degrees:F2}°)");
ed.WriteMessage($"\nSigned angle: {signedAngle:F4} radians");

// Practical example: find angle between two lines
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(10, 10, 0);

Vector3d line1 = p1.GetVectorTo(p2);
Vector3d line2 = p2.GetVectorTo(p3);
double lineAngle = line1.GetAngleTo(line2);

ed.WriteMessage($"\nAngle between lines: {lineAngle * (180.0 / Math.PI):F2}°");
```

### Example 5: Vector Transformations
```csharp
Vector3d vec = new Vector3d(1, 0, 0);

// Rotate vector 90° around Z axis
double angle = Math.PI / 2;
Vector3d rotated = vec.RotateBy(angle, Vector3d.ZAxis);
// Result: (0, 1, 0)

// Transform by matrix
Matrix3d rotation = Matrix3d.Rotation(Math.PI / 4, Vector3d.ZAxis, Point3d.Origin);
Vector3d transformed = vec.TransformBy(rotation);

// Scale vector
Vector3d scaled = vec.MultiplyBy(5.0); // (5, 0, 0)

ed.WriteMessage($"\nOriginal: {vec}");
ed.WriteMessage($"\nRotated 90°: ({rotated.X:F2}, {rotated.Y:F2}, {rotated.Z:F2})");
ed.WriteMessage($"\nTransformed: ({transformed.X:F2}, {transformed.Y:F2}, {transformed.Z:F2})");
```

### Example 6: Perpendicular Vector Generation
```csharp
Vector3d vec = new Vector3d(1, 1, 0);

// Get perpendicular vector using cross product
Vector3d perp1 = vec.CrossProduct(Vector3d.ZAxis);
perp1 = perp1.GetNormal(); // Normalize

// Alternative: rotate 90°
Vector3d perp2 = vec.RotateBy(Math.PI / 2, Vector3d.ZAxis);
perp2 = perp2.GetNormal();

// Verify perpendicularity
double dot = vec.DotProduct(perp1);
bool isPerpendicular = Math.Abs(dot) < 0.001;

ed.WriteMessage($"\nOriginal vector: {vec}");
ed.WriteMessage($"\nPerpendicular vector: {perp1}");
ed.WriteMessage($"\nDot product (should be ~0): {dot:F6}");
ed.WriteMessage($"\nIs perpendicular: {isPerpendicular}");
```

### Example 7: Vector Projection
```csharp
// Project vector A onto vector B
Vector3d vecA = new Vector3d(5, 3, 0);
Vector3d vecB = new Vector3d(1, 0, 0); // X axis

// Projection formula: proj_B(A) = (A · B / |B|²) * B
double dotProduct = vecA.DotProduct(vecB);
double lengthSqrd = vecB.LengthSqrd;
Vector3d projection = vecB * (dotProduct / lengthSqrd);

// Get perpendicular component (rejection)
Vector3d rejection = vecA - projection;

ed.WriteMessage($"\nVector A: {vecA}");
ed.WriteMessage($"\nVector B: {vecB}");
ed.WriteMessage($"\nProjection of A onto B: {projection}");
ed.WriteMessage($"\nRejection (perpendicular): {rejection}");

// Verify: projection + rejection = original
Vector3d sum = projection + rejection;
ed.WriteMessage($"\nVerification (should equal A): {sum}");
```

### Example 8: Checking Vector Relationships
```csharp
Vector3d v1 = new Vector3d(1, 0, 0);
Vector3d v2 = new Vector3d(2, 0, 0); // Parallel to v1
Vector3d v3 = new Vector3d(0, 1, 0); // Perpendicular to v1
Vector3d v4 = new Vector3d(-1, 0, 0); // Opposite to v1

// Check parallel
bool isParallel = v1.IsParallelTo(v2); // true

// Check perpendicular
bool isPerpendicular = v1.IsPerpendicularTo(v3); // true

// Check codirectional (same direction)
bool isCodirectional = v1.IsCodirectionalTo(v2); // true
bool isCodirectional2 = v1.IsCodirectionalTo(v4); // false (opposite)

// Check equality with tolerance
Tolerance tol = new Tolerance(0.001, 0.001);
Vector3d v5 = new Vector3d(1.0001, 0, 0);
bool isEqual = v1.IsEqualTo(v5, tol); // true

ed.WriteMessage($"\nv1 parallel to v2: {isParallel}");
ed.WriteMessage($"\nv1 perpendicular to v3: {isPerpendicular}");
ed.WriteMessage($"\nv1 codirectional to v2: {isCodirectional}");
ed.WriteMessage($"\nv1 codirectional to v4: {isCodirectional2}");
```

### Example 9: Finding Nearest Entity from a Coordinate
```csharp
[CommandMethod("FINDNEAREST")]
public void FindNearestEntity()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt for point
    PromptPointResult ppr = ed.GetPoint("\nSelect search point: ");
    if (ppr.Status != PromptStatus.OK) return;
    
    Point3d searchPoint = ppr.Value;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
        
        Entity nearestEntity = null;
        double minDistance = double.MaxValue;
        Point3d nearestPoint = Point3d.Origin;
        
        foreach (ObjectId id in btr)
        {
            Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
            if (ent == null) continue;
            
            // Get closest point on entity
            Point3d closestPt = ent.GetClosestPointTo(searchPoint, false);
            
            // Calculate distance using vector
            Vector3d distanceVec = searchPoint.GetVectorTo(closestPt);
            double distance = distanceVec.Length;
            
            if (distance < minDistance)
            {
                minDistance = distance;
                nearestEntity = ent;
                nearestPoint = closestPt;
            }
        }
        
        if (nearestEntity != null)
        {
            // Calculate direction vector
            Vector3d direction = searchPoint.GetVectorTo(nearestPoint);
            Vector3d unitDirection = direction.GetNormal();
            
            ed.WriteMessage($"\n--- Nearest Entity Found ---");
            ed.WriteMessage($"\nEntity Type: {nearestEntity.GetType().Name}");
            ed.WriteMessage($"\nDistance: {minDistance:F4}");
            ed.WriteMessage($"\nNearest Point: ({nearestPoint.X:F2}, {nearestPoint.Y:F2}, {nearestPoint.Z:F2})");
            ed.WriteMessage($"\nDirection Vector: ({direction.X:F2}, {direction.Y:F2}, {direction.Z:F2})");
            ed.WriteMessage($"\nUnit Direction: ({unitDirection.X:F4}, {unitDirection.Y:F4}, {unitDirection.Z:F4})");
            
            // Highlight the entity
            nearestEntity.Highlight();
            
            // Draw a line from search point to nearest point
            Line indicator = new Line(searchPoint, nearestPoint);
            indicator.ColorIndex = 1; // Red
            
            BlockTableRecord space = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
            space.AppendEntity(indicator);
            tr.AddNewlyCreatedDBObject(indicator, true);
        }
        else
        {
            ed.WriteMessage("\nNo entities found in current space.");
        }
        
        tr.Commit();
    }
}
```

### Example 10: Creating Offset Lines Using Vectors
```csharp
// Create offset line perpendicular to original
Point3d start = new Point3d(0, 0, 0);
Point3d end = new Point3d(10, 0, 0);
double offsetDistance = 5.0;

// Get line direction vector
Vector3d lineDirection = start.GetVectorTo(end);

// Get perpendicular vector (rotate 90° around Z)
Vector3d perpendicular = lineDirection.RotateBy(Math.PI / 2, Vector3d.ZAxis);
Vector3d offsetVector = perpendicular.GetNormal() * offsetDistance;

// Create offset points
Point3d offsetStart = start.Add(offsetVector);
Point3d offsetEnd = end.Add(offsetVector);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Original line
    Line originalLine = new Line(start, end);
    originalLine.ColorIndex = 7; // White
    btr.AppendEntity(originalLine);
    tr.AddNewlyCreatedDBObject(originalLine, true);
    
    // Offset line
    Line offsetLine = new Line(offsetStart, offsetEnd);
    offsetLine.ColorIndex = 3; // Green
    btr.AppendEntity(offsetLine);
    tr.AddNewlyCreatedDBObject(offsetLine, true);
    
    tr.Commit();
}

ed.WriteMessage($"\nOriginal line: {start} to {end}");
ed.WriteMessage($"\nOffset line: {offsetStart} to {offsetEnd}");
ed.WriteMessage($"\nOffset vector: {offsetVector}");
```

## Common Patterns

### Getting Direction Between Points
```csharp
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 10, 0);

Vector3d direction = pt1.GetVectorTo(pt2);
Vector3d unitDirection = direction.GetNormal();
double distance = direction.Length;
```

### Creating Normal Vector for a Plane
```csharp
// Three points define a plane
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(0, 10, 0);

Vector3d v1 = p1.GetVectorTo(p2);
Vector3d v2 = p1.GetVectorTo(p3);

// Normal is perpendicular to both vectors
Vector3d normal = v1.CrossProduct(v2).GetNormal();
```

### Checking if Point is Left or Right of Line
```csharp
Point3d lineStart = new Point3d(0, 0, 0);
Point3d lineEnd = new Point3d(10, 0, 0);
Point3d testPoint = new Point3d(5, 5, 0);

Vector3d lineVec = lineStart.GetVectorTo(lineEnd);
Vector3d pointVec = lineStart.GetVectorTo(testPoint);

// Cross product Z component determines side
Vector3d cross = lineVec.CrossProduct(pointVec);
if (cross.Z > 0)
    ed.WriteMessage("\nPoint is on the left");
else if (cross.Z < 0)
    ed.WriteMessage("\nPoint is on the right");
else
    ed.WriteMessage("\nPoint is on the line");
```

## Best Practices

1. **Normalization**: Always normalize vectors when you need direction only: `vec.GetNormal()`
2. **Performance**: Use `LengthSqrd` instead of `Length` when comparing distances
3. **Immutability**: Vector3d is a struct; operations return new vectors
4. **Tolerance**: Use tolerance-based comparisons for floating-point vectors
5. **Zero Vectors**: Check for zero-length vectors before normalizing
6. **Cross Product**: Remember that cross product order matters: `A × B ≠ B × A`
7. **Dot Product**: Use for angle calculations and projections
8. **Unit Vectors**: Use predefined `XAxis`, `YAxis`, `ZAxis` for clarity

## Related Classes
- **Point3d** - 3D point in space
- **Matrix3d** - Transformation matrices
- **Plane** - Plane definition using point and normal
- **Vector2d** - 2D vector for planar operations
- **Line3d** - Unbounded 3D line
- **Tolerance** - Tolerance for comparisons

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Vector3d Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Vector3d)
- [Geometry Namespace](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
