# Matrix3d Struct

## Overview
The `Matrix3d` struct represents a 4x4 transformation matrix used for geometric transformations in 3D space. It is essential for translation, rotation, scaling, mirroring, and coordinate system conversions in AutoCAD.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `CoordinateSystem3d` | `CoordinateSystem3d` | Gets the coordinate system represented by the matrix |
| `Inverse` | `Matrix3d` | Gets the inverse matrix |
| `IsIdentity` | `bool` | Checks if matrix is identity matrix |
| `IsUniscaledOrtho` | `bool` | Checks if matrix is orthogonal with uniform scaling |
| `Norm` | `double` | Gets the norm of the matrix |

## Static Factory Methods

| Method | Description |
|--------|-------------|
| `Identity` | Returns identity matrix (no transformation) |
| `Displacement(Vector3d)` | Creates translation matrix |
| `Rotation(double, Vector3d, Point3d)` | Creates rotation matrix around axis |
| `Scaling(double, Point3d)` | Creates uniform scaling matrix |
| `Scaling(Vector3d, Point3d)` | Creates non-uniform scaling matrix |
| `Mirroring(Plane)` | Creates mirror transformation across plane |
| `Mirroring(Point3d)` | Creates mirror transformation across point |
| `Mirroring(Line3d)` | Creates mirror transformation across line |
| `AlignCoordinateSystem(...)` | Creates coordinate system alignment matrix |
| `PlaneToWorld(Plane)` | Creates plane-to-world transformation |
| `WorldToPlane(Plane)` | Creates world-to-plane transformation |
| `Projection(Plane, Vector3d)` | Creates projection matrix onto plane |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `PreMultiplyBy(Matrix3d)` | `Matrix3d` | Multiplies this matrix by another (this * other) |
| `PostMultiplyBy(Matrix3d)` | `Matrix3d` | Multiplies another matrix by this (other * this) |
| `Transpose()` | `Matrix3d` | Returns transposed matrix |
| `Invert()` | `Matrix3d` | Returns inverted matrix |
| `GetCoordinateSystem()` | `CoordinateSystem3d` | Gets coordinate system from matrix |
| `IsEqualTo(Matrix3d)` | `bool` | Checks equality with default tolerance |
| `IsEqualTo(Matrix3d, Tolerance)` | `bool` | Checks equality with custom tolerance |

## Code Examples

### Example 1: Translation (Displacement)
```csharp
// Move entities 10 units in X, 5 in Y
Vector3d displacement = new Vector3d(10, 5, 0);
Matrix3d translation = Matrix3d.Displacement(displacement);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create a circle
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    circle.SetDatabaseDefaults();
    
    // Apply translation
    circle.TransformBy(translation);
    
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    tr.Commit();
}

ed.WriteMessage($"\nTranslation matrix created: {displacement}");
ed.WriteMessage($"\nCircle moved to: {circle.Center}");
```

### Example 2: Rotation
```csharp
// Rotate 45° around Z axis at origin
double angle = Math.PI / 4; // 45 degrees
Vector3d axis = Vector3d.ZAxis;
Point3d basePoint = Point3d.Origin;

Matrix3d rotation = Matrix3d.Rotation(angle, axis, basePoint);

// Apply to a point
Point3d pt = new Point3d(10, 0, 0);
Point3d rotatedPt = pt.TransformBy(rotation);

ed.WriteMessage($"\nOriginal point: ({pt.X:F2}, {pt.Y:F2}, {pt.Z:F2})");
ed.WriteMessage($"\nRotated 45°: ({rotatedPt.X:F2}, {rotatedPt.Y:F2}, {rotatedPt.Z:F2})");

// Rotate entity around custom point
Point3d rotationCenter = new Point3d(5, 5, 0);
Matrix3d rotation90 = Matrix3d.Rotation(Math.PI / 2, Vector3d.ZAxis, rotationCenter);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get entity and rotate
    Entity ent = tr.GetObject(selectedId, OpenMode.ForWrite) as Entity;
    ent.TransformBy(rotation90);
    tr.Commit();
}
```

### Example 3: Scaling
```csharp
// Uniform scaling (2x)
Point3d scaleBase = Point3d.Origin;
Matrix3d uniformScale = Matrix3d.Scaling(2.0, scaleBase);

// Non-uniform scaling (2x in X, 3x in Y, 1x in Z)
Vector3d scaleFactors = new Vector3d(2.0, 3.0, 1.0);
Matrix3d nonUniformScale = Matrix3d.Scaling(scaleFactors, scaleBase);

// Apply to entity
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = new Circle(new Point3d(10, 10, 0), Vector3d.ZAxis, 5.0);
    circle.SetDatabaseDefaults();
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    // Scale the circle
    circle.TransformBy(uniformScale);
    
    tr.Commit();
}

ed.WriteMessage($"\nUniform scale factor: 2.0");
ed.WriteMessage($"\nNon-uniform scale: X={scaleFactors.X}, Y={scaleFactors.Y}, Z={scaleFactors.Z}");
```

### Example 4: Mirroring
```csharp
// Mirror across YZ plane (X = 0)
Point3d planePoint = Point3d.Origin;
Vector3d planeNormal = Vector3d.XAxis;
Plane mirrorPlane = new Plane(planePoint, planeNormal);

Matrix3d mirror = Matrix3d.Mirroring(mirrorPlane);

// Apply to point
Point3d pt = new Point3d(10, 5, 0);
Point3d mirroredPt = pt.TransformBy(mirror);

ed.WriteMessage($"\nOriginal: ({pt.X}, {pt.Y}, {pt.Z})");
ed.WriteMessage($"\nMirrored: ({mirroredPt.X}, {mirroredPt.Y}, {mirroredPt.Z})");

// Mirror entity across custom line
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Define mirror line
    Point3d pt1 = new Point3d(0, 0, 0);
    Point3d pt2 = new Point3d(10, 10, 0);
    
    // Create plane perpendicular to XY through the line
    Vector3d lineVec = pt1.GetVectorTo(pt2);
    Vector3d perpVec = lineVec.CrossProduct(Vector3d.ZAxis).GetNormal();
    Plane plane = new Plane(pt1, perpVec);
    
    Matrix3d mirrorMatrix = Matrix3d.Mirroring(plane);
    
    // Apply to entity
    Entity ent = tr.GetObject(selectedId, OpenMode.ForWrite) as Entity;
    ent.TransformBy(mirrorMatrix);
    
    tr.Commit();
}
```

### Example 5: Combining Transformations
```csharp
// Move, then rotate, then scale
Vector3d moveVec = new Vector3d(10, 5, 0);
Matrix3d move = Matrix3d.Displacement(moveVec);

double rotAngle = Math.PI / 4;
Matrix3d rotate = Matrix3d.Rotation(rotAngle, Vector3d.ZAxis, Point3d.Origin);

double scaleFactor = 2.0;
Matrix3d scale = Matrix3d.Scaling(scaleFactor, Point3d.Origin);

// Combine: order matters! (scale * rotate * move)
Matrix3d combined = move.PreMultiplyBy(rotate).PreMultiplyBy(scale);

// Alternative using PostMultiplyBy
Matrix3d combined2 = scale.PostMultiplyBy(rotate).PostMultiplyBy(move);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = new Circle(Point3d.Origin, Vector3d.ZAxis, 5.0);
    circle.SetDatabaseDefaults();
    
    // Apply combined transformation
    circle.TransformBy(combined);
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}

ed.WriteMessage("\nCombined transformation applied: Scale -> Rotate -> Move");
```

### Example 6: Coordinate System Alignment
```csharp
// Align from one coordinate system to another
Point3d fromOrigin = Point3d.Origin;
Vector3d fromXAxis = Vector3d.XAxis;
Vector3d fromYAxis = Vector3d.YAxis;
Vector3d fromZAxis = Vector3d.ZAxis;

Point3d toOrigin = new Point3d(10, 10, 0);
Vector3d toXAxis = new Vector3d(0, 1, 0); // Rotated 90°
Vector3d toYAxis = new Vector3d(-1, 0, 0);
Vector3d toZAxis = Vector3d.ZAxis;

Matrix3d alignment = Matrix3d.AlignCoordinateSystem(
    fromOrigin, fromXAxis, fromYAxis, fromZAxis,
    toOrigin, toXAxis, toYAxis, toZAxis
);

// Apply to entity
Point3d testPt = new Point3d(5, 0, 0);
Point3d alignedPt = testPt.TransformBy(alignment);

ed.WriteMessage($"\nOriginal point: {testPt}");
ed.WriteMessage($"\nAligned point: {alignedPt}");
```

### Example 7: Plane Transformations
```csharp
// Transform from world coordinates to plane coordinates
Point3d planeOrigin = new Point3d(10, 10, 0);
Vector3d planeNormal = new Vector3d(0, 0, 1);
Plane workPlane = new Plane(planeOrigin, planeNormal);

// World to plane
Matrix3d worldToPlane = Matrix3d.WorldToPlane(workPlane);

// Plane to world
Matrix3d planeToWorld = Matrix3d.PlaneToWorld(workPlane);

// Transform a point from world to plane coordinates
Point3d worldPt = new Point3d(15, 15, 0);
Point3d planePt = worldPt.TransformBy(worldToPlane);

ed.WriteMessage($"\nWorld point: {worldPt}");
ed.WriteMessage($"\nPlane coordinates: {planePt}");

// Transform back
Point3d backToWorld = planePt.TransformBy(planeToWorld);
ed.WriteMessage($"\nBack to world: {backToWorld}");
```

### Example 8: Inverse Transformations
```csharp
// Create a transformation
Vector3d displacement = new Vector3d(10, 5, 3);
Matrix3d transform = Matrix3d.Displacement(displacement);

// Get inverse (undo the transformation)
Matrix3d inverse = transform.Inverse;

// Apply transformation then inverse
Point3d original = new Point3d(5, 5, 5);
Point3d transformed = original.TransformBy(transform);
Point3d backToOriginal = transformed.TransformBy(inverse);

ed.WriteMessage($"\nOriginal: {original}");
ed.WriteMessage($"\nTransformed: {transformed}");
ed.WriteMessage($"\nBack to original: {backToOriginal}");

// Verify they're equal
bool isEqual = original.IsEqualTo(backToOriginal);
ed.WriteMessage($"\nPoints equal: {isEqual}");
```

### Example 9: Checking Matrix Properties
```csharp
Matrix3d identity = Matrix3d.Identity;
Matrix3d translation = Matrix3d.Displacement(new Vector3d(5, 5, 0));
Matrix3d rotation = Matrix3d.Rotation(Math.PI / 4, Vector3d.ZAxis, Point3d.Origin);

// Check if identity
bool isIdentity1 = identity.IsIdentity; // true
bool isIdentity2 = translation.IsIdentity; // false

// Check if uniscaled orthogonal
bool isOrtho1 = rotation.IsUniscaledOrtho; // true (rotation preserves angles and lengths)
bool isOrtho2 = translation.IsUniscaledOrtho; // true (translation preserves everything)

ed.WriteMessage($"\nIdentity matrix is identity: {isIdentity1}");
ed.WriteMessage($"\nTranslation is identity: {isIdentity2}");
ed.WriteMessage($"\nRotation is uniscaled ortho: {isOrtho1}");
ed.WriteMessage($"\nTranslation is uniscaled ortho: {isOrtho2}");

// Get coordinate system from matrix
CoordinateSystem3d cs = rotation.CoordinateSystem3d;
ed.WriteMessage($"\nCoordinate system origin: {cs.Origin}");
```

### Example 10: Practical - Copy and Transform Entities
```csharp
[CommandMethod("COPYROTATE")]
public void CopyAndRotate()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select entity
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect entity to copy and rotate: ");
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    // Get base point
    PromptPointOptions ppo = new PromptPointOptions("\nSpecify rotation center: ");
    PromptPointResult ppr = ed.GetPoint(ppo);
    if (ppr.Status != PromptStatus.OK) return;
    Point3d basePoint = ppr.Value;
    
    // Get number of copies
    PromptIntegerOptions pio = new PromptIntegerOptions("\nNumber of copies: ");
    pio.DefaultValue = 8;
    PromptIntegerResult pir = ed.GetInteger(pio);
    if (pir.Status != PromptStatus.OK) return;
    int numCopies = pir.Value;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        Entity sourceEnt = tr.GetObject(per.ObjectId, OpenMode.ForRead) as Entity;
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
        
        double angleIncrement = (2 * Math.PI) / numCopies;
        
        for (int i = 1; i < numCopies; i++)
        {
            // Create copy
            Entity copy = sourceEnt.Clone() as Entity;
            
            // Create rotation matrix
            double angle = angleIncrement * i;
            Matrix3d rotation = Matrix3d.Rotation(angle, Vector3d.ZAxis, basePoint);
            
            // Apply transformation
            copy.TransformBy(rotation);
            
            // Add to database
            btr.AppendEntity(copy);
            tr.AddNewlyCreatedDBObject(copy, true);
        }
        
        tr.Commit();
        ed.WriteMessage($"\nCreated {numCopies - 1} rotated copies");
    }
}
```

## Common Patterns

### Move Entity to Origin
```csharp
Point3d currentPosition = entity.GeometricExtents.MinPoint;
Vector3d toOrigin = currentPosition.GetVectorTo(Point3d.Origin);
Matrix3d move = Matrix3d.Displacement(toOrigin);
entity.TransformBy(move);
```

### Rotate Around Entity's Center
```csharp
Point3d center = entity.GeometricExtents.MinPoint + 
    ((entity.GeometricExtents.MaxPoint - entity.GeometricExtents.MinPoint) * 0.5);
Matrix3d rotation = Matrix3d.Rotation(angle, Vector3d.ZAxis, center);
entity.TransformBy(rotation);
```

### Scale from Entity's Center
```csharp
Point3d center = entity.GeometricExtents.MinPoint + 
    ((entity.GeometricExtents.MaxPoint - entity.GeometricExtents.MinPoint) * 0.5);
Matrix3d scale = Matrix3d.Scaling(scaleFactor, center);
entity.TransformBy(scale);
```

## Transformation Order

**CRITICAL**: Matrix multiplication order matters!

```csharp
// Using PreMultiplyBy: applies right-to-left
Matrix3d result = move.PreMultiplyBy(rotate).PreMultiplyBy(scale);
// Equivalent to: scale * rotate * move
// Entity is: moved, then rotated, then scaled

// Using PostMultiplyBy: applies left-to-right
Matrix3d result = scale.PostMultiplyBy(rotate).PostMultiplyBy(move);
// Equivalent to: scale * rotate * move
// Same result as above
```

## Best Practices

1. **Transformation Order**: Be mindful of transformation order - it affects the result
2. **Use Factory Methods**: Use static factory methods instead of constructing matrices manually
3. **Combine Transformations**: Combine multiple transformations into one matrix for efficiency
4. **Inverse Matrices**: Use `Inverse` property to undo transformations
5. **Identity Check**: Use `IsIdentity` to check if transformation does nothing
6. **Coordinate Systems**: Use `AlignCoordinateSystem` for complex coordinate conversions
7. **Performance**: Apply combined matrix once rather than multiple transformations
8. **Immutability**: Matrix3d is a struct; operations return new matrices

## Related Classes
- **Vector3d** - Direction and magnitude for displacement
- **Point3d** - Points to transform
- **Plane** - Plane for mirroring and projections
- **CoordinateSystem3d** - Coordinate system representation
- **Entity** - All entities have `TransformBy()` method
- **Matrix2d** - 2D transformation matrix

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Matrix3d Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Matrix3d)
- [Geometry Namespace](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
