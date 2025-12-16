# Point3d Struct

## Overview
The `Point3d` struct represents a point in 3D space with X, Y, and Z coordinates. It is one of the most fundamental geometry classes in the AutoCAD .NET API, used extensively for positioning entities, defining vertices, and performing geometric calculations.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `X` | `double` | Gets the X coordinate |
| `Y` | `double` | Gets the Y coordinate |
| `Z` | `double` | Gets the Z coordinate |
| `Origin` | `Point3d` (static) | Gets the origin point (0, 0, 0) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Add(Vector3d)` | `Point3d` | Adds a vector to the point |
| `Subtract(Vector3d)` | `Point3d` | Subtracts a vector from the point |
| `GetVectorTo(Point3d)` | `Vector3d` | Gets the vector from this point to another |
| `DistanceTo(Point3d)` | `double` | Calculates distance to another point |
| `TransformBy(Matrix3d)` | `Point3d` | Transforms the point by a matrix |
| `RotateBy(double, Vector3d, Point3d)` | `Point3d` | Rotates the point around an axis |
| `Mirror(Plane)` | `Point3d` | Mirrors the point across a plane |
| `ToArray()` | `double[]` | Converts to array [X, Y, Z] |

## Code Examples

### Example 1: Creating Points
```csharp
// Create point at origin
Point3d origin = Point3d.Origin; // (0, 0, 0)

// Create point with coordinates
Point3d pt1 = new Point3d(10, 20, 0);
Point3d pt2 = new Point3d(5.5, 7.3, 12.8);

// Create from array
double[] coords = { 15, 25, 35 };
Point3d pt3 = new Point3d(coords);

ed.WriteMessage($"\nPoint 1: ({pt1.X}, {pt1.Y}, {pt1.Z})");
ed.WriteMessage($"\nPoint 2: ({pt2.X}, {pt2.Y}, {pt2.Z})");
ed.WriteMessage($"\nPoint 3: ({pt3.X}, {pt3.Y}, {pt3.Z})");
```

### Example 2: Point Arithmetic
```csharp
Point3d pt = new Point3d(10, 10, 0);
Vector3d vec = new Vector3d(5, 3, 0);

// Add vector to point
Point3d newPt = pt.Add(vec); // (15, 13, 0)

// Subtract vector from point
Point3d movedPt = pt.Subtract(vec); // (5, 7, 0)

// Get vector between two points
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 10, 0);
Vector3d direction = pt1.GetVectorTo(pt2);

ed.WriteMessage($"\nOriginal: {pt}");
ed.WriteMessage($"\nAfter add: {newPt}");
ed.WriteMessage($"\nAfter subtract: {movedPt}");
ed.WriteMessage($"\nDirection vector: {direction}");
```

### Example 3: Distance Calculations
```csharp
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(10, 0, 0);
Point3d pt3 = new Point3d(0, 10, 0);
Point3d pt4 = new Point3d(10, 10, 10);

// Calculate distances
double dist12 = pt1.DistanceTo(pt2); // 10.0
double dist13 = pt1.DistanceTo(pt3); // 10.0
double dist14 = pt1.DistanceTo(pt4); // 17.32 (3D distance)

ed.WriteMessage($"\nDistance pt1 to pt2: {dist12:F2}");
ed.WriteMessage($"\nDistance pt1 to pt3: {dist13:F2}");
ed.WriteMessage($"\nDistance pt1 to pt4: {dist14:F2}");
```

### Example 4: Midpoint Calculation
```csharp
Point3d pt1 = new Point3d(0, 0, 0);
Point3d pt2 = new Point3d(20, 10, 5);

// Calculate midpoint
Point3d midpoint = new Point3d(
    (pt1.X + pt2.X) / 2,
    (pt1.Y + pt2.Y) / 2,
    (pt1.Z + pt2.Z) / 2
);

// Alternative using vector math
Vector3d halfVector = pt1.GetVectorTo(pt2) * 0.5;
Point3d midpoint2 = pt1.Add(halfVector);

ed.WriteMessage($"\nMidpoint: ({midpoint.X}, {midpoint.Y}, {midpoint.Z})");
```

### Example 5: Transforming Points
```csharp
Point3d pt = new Point3d(10, 0, 0);

// Create translation matrix (move 5 units in X, 10 in Y)
Matrix3d translation = Matrix3d.Displacement(new Vector3d(5, 10, 0));
Point3d translatedPt = pt.TransformBy(translation);

// Create rotation matrix (rotate 90° around Z axis)
Matrix3d rotation = Matrix3d.Rotation(Math.PI / 2, Vector3d.ZAxis, Point3d.Origin);
Point3d rotatedPt = pt.TransformBy(rotation);

// Create scaling matrix (scale by 2)
Matrix3d scaling = Matrix3d.Scaling(2.0, Point3d.Origin);
Point3d scaledPt = pt.TransformBy(scaling);

ed.WriteMessage($"\nOriginal: {pt}");
ed.WriteMessage($"\nTranslated: {translatedPt}");
ed.WriteMessage($"\nRotated: {rotatedPt}");
ed.WriteMessage($"\nScaled: {scaledPt}");
```

### Example 6: Rotating Points
```csharp
Point3d pt = new Point3d(10, 0, 0);
Point3d center = Point3d.Origin;

// Rotate 45° around Z axis
double angle = Math.PI / 4; // 45 degrees in radians
Point3d rotated = pt.RotateBy(angle, Vector3d.ZAxis, center);

ed.WriteMessage($"\nOriginal: ({pt.X:F2}, {pt.Y:F2}, {pt.Z:F2})");
ed.WriteMessage($"\nRotated 45°: ({rotated.X:F2}, {rotated.Y:F2}, {rotated.Z:F2})");

// Rotate multiple times to create circular pattern
for (int i = 0; i < 8; i++)
{
    double ang = (i * 2 * Math.PI) / 8;
    Point3d circPt = pt.RotateBy(ang, Vector3d.ZAxis, center);
    ed.WriteMessage($"\nPoint {i}: ({circPt.X:F2}, {circPt.Y:F2})");
}
```

### Example 7: Creating Entities with Points
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create line using two points
    Point3d startPt = new Point3d(0, 0, 0);
    Point3d endPt = new Point3d(10, 10, 0);
    
    Line line = new Line(startPt, endPt);
    line.SetDatabaseDefaults();
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    // Create circle using center point
    Point3d center = new Point3d(5, 5, 0);
    Circle circle = new Circle(center, Vector3d.ZAxis, 3.0);
    circle.SetDatabaseDefaults();
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    // Create arc using center, start, and end points
    Point3d arcCenter = new Point3d(0, 0, 0);
    double radius = 5.0;
    double startAngle = 0;
    double endAngle = Math.PI / 2;
    
    Arc arc = new Arc(arcCenter, Vector3d.ZAxis, radius, startAngle, endAngle);
    arc.SetDatabaseDefaults();
    btr.AppendEntity(arc);
    tr.AddNewlyCreatedDBObject(arc, true);
    
    tr.Commit();
}
```

### Example 8: Point Array Conversion
```csharp
Point3d pt = new Point3d(10, 20, 30);

// Convert to array
double[] coords = pt.ToArray();

ed.WriteMessage("\nPoint coordinates:");
ed.WriteMessage($"\n  X: {coords[0]}");
ed.WriteMessage($"\n  Y: {coords[1]}");
ed.WriteMessage($"\n  Z: {coords[2]}");

// Create point from array
Point3d newPt = new Point3d(coords);

ed.WriteMessage($"\nRecreated point: ({newPt.X}, {newPt.Y}, {newPt.Z})");
```

### Example 9: Checking Point Equality
```csharp
Point3d pt1 = new Point3d(10, 20, 0);
Point3d pt2 = new Point3d(10, 20, 0);
Point3d pt3 = new Point3d(10.0001, 20, 0);

// Exact equality
bool exact = pt1 == pt2; // true
bool notExact = pt1 == pt3; // false

// Tolerance-based equality
Tolerance tol = new Tolerance(0.001, 0.001);
bool withinTolerance = pt1.IsEqualTo(pt3, tol); // true if difference < 0.001

ed.WriteMessage($"\nExact equality (pt1 == pt2): {exact}");
ed.WriteMessage($"\nExact equality (pt1 == pt3): {notExact}");
ed.WriteMessage($"\nWithin tolerance: {withinTolerance}");
```

### Example 10: Finding Closest Point on Line
```csharp
// Define a line
Point3d lineStart = new Point3d(0, 0, 0);
Point3d lineEnd = new Point3d(10, 0, 0);

// Point to project
Point3d testPoint = new Point3d(5, 5, 0);

// Calculate closest point on line
Vector3d lineVector = lineStart.GetVectorTo(lineEnd);
Vector3d pointVector = lineStart.GetVectorTo(testPoint);

double dotProduct = lineVector.DotProduct(pointVector);
double lineLength = lineVector.Length;
double t = dotProduct / (lineLength * lineLength);

// Clamp t to [0, 1] to stay on line segment
t = Math.Max(0, Math.Min(1, t));

Point3d closestPoint = lineStart.Add(lineVector * t);

ed.WriteMessage($"\nTest point: {testPoint}");
ed.WriteMessage($"\nClosest point on line: {closestPoint}");
ed.WriteMessage($"\nDistance: {testPoint.DistanceTo(closestPoint):F2}");
```

## Common Patterns

### Creating a Grid of Points
```csharp
List<Point3d> gridPoints = new List<Point3d>();

for (int x = 0; x < 10; x++)
{
    for (int y = 0; y < 10; y++)
    {
        gridPoints.Add(new Point3d(x * 10, y * 10, 0));
    }
}
```

### Interpolating Between Points
```csharp
Point3d start = new Point3d(0, 0, 0);
Point3d end = new Point3d(10, 10, 0);

// Get 10 points along the line
for (int i = 0; i <= 10; i++)
{
    double t = i / 10.0;
    Vector3d vec = start.GetVectorTo(end) * t;
    Point3d interpolated = start.Add(vec);
}
```

## Point3d vs Point2d

| Feature | Point3d | Point2d |
|---------|---------|---------|
| Dimensions | X, Y, Z | X, Y |
| Namespace | Geometry | Geometry |
| Use Case | 3D coordinates | 2D coordinates |
| Conversion | Can convert to Point2d | Can convert to Point3d (Z=0) |

## Best Practices

1. **Use Origin**: Use `Point3d.Origin` instead of `new Point3d(0, 0, 0)`
2. **Immutability**: Point3d is a struct; operations return new points
3. **Tolerance**: Use tolerance-based equality for floating-point comparisons
4. **Vector Math**: Use `GetVectorTo()` for direction and distance calculations
5. **Transformations**: Use `TransformBy()` for complex geometric operations
6. **2D Operations**: Set Z=0 for 2D operations in XY plane
7. **Performance**: Point3d is a value type (struct), passed by value
8. **Null Checks**: Point3d cannot be null (it's a struct)

## Related Classes
- **Vector3d** - Represents direction and magnitude
- **Point2d** - 2D point (X, Y only)
- **Matrix3d** - Transformation matrix
- **Line3d** - 3D line using two points
- **Plane** - Plane defined by point and normal
- **Extents3d** - Bounding box using min/max points

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Point3d Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Point3d)
- [Geometry Namespace](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
