# Plane Struct

## Overview
The `Plane` struct represents an infinite plane in 3D space, defined by a point on the plane and a normal vector perpendicular to it. Planes are used for mirroring, projections, coordinate system definitions, and geometric calculations.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Normal` | `Vector3d` | Gets the normal vector perpendicular to the plane |
| `PointOnPlane` | `Point3d` | Gets a point on the plane |

## Constructors

| Constructor | Description |
|-------------|-------------|
| `Plane(Point3d, Vector3d)` | Creates plane from point and normal vector |
| `Plane(Point3d, Point3d, Point3d)` | Creates plane from three non-collinear points |
| `Plane()` | Creates XY plane at origin |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d)` | `Point3d` | Gets closest point on plane to given point |
| `DistanceTo(Point3d)` | `double` | Gets signed distance from point to plane |
| `IsCoplanarTo(Plane)` | `bool` | Checks if coplanar with another plane |
| `IsPerpendicular(Plane)` | `bool` | Checks if perpendicular to another plane |
| `IsParallelTo(Plane)` | `bool` | Checks if parallel to another plane |
| `IsEqualTo(Plane)` | `bool` | Checks equality with default tolerance |
| `IsEqualTo(Plane, Tolerance)` | `bool` | Checks equality with custom tolerance |

## Code Examples

### Example 1: Creating Planes
```csharp
// XY plane at origin (Z = 0)
Plane xyPlane = new Plane();

// Plane from point and normal
Point3d origin = Point3d.Origin;
Vector3d normal = Vector3d.ZAxis;
Plane customPlane = new Plane(origin, normal);

// Plane from three points
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(0, 10, 0);
Plane planeFrom3Pts = new Plane(p1, p2, p3);

ed.WriteMessage($"\nXY Plane normal: {xyPlane.Normal}");
ed.WriteMessage($"\nCustom plane point: {customPlane.PointOnPlane}");
ed.WriteMessage($"\n3-point plane normal: {planeFrom3Pts.Normal}");
```

### Example 2: Point-to-Plane Distance
```csharp
// Create XY plane at Z = 0
Plane xyPlane = new Plane(Point3d.Origin, Vector3d.ZAxis);

// Test points
Point3d pt1 = new Point3d(5, 5, 10); // Above plane
Point3d pt2 = new Point3d(3, 3, -5); // Below plane
Point3d pt3 = new Point3d(7, 7, 0); // On plane

// Calculate signed distances
double dist1 = xyPlane.DistanceTo(pt1); // +10
double dist2 = xyPlane.DistanceTo(pt2); // -5
double dist3 = xyPlane.DistanceTo(pt3); // 0

ed.WriteMessage($"\nDistance pt1 to plane: {dist1:F2}");
ed.WriteMessage($"\nDistance pt2 to plane: {dist2:F2}");
ed.WriteMessage($"\nDistance pt3 to plane: {dist3:F2}");

// Positive = above plane (in direction of normal)
// Negative = below plane (opposite to normal)
// Zero = on plane
```

### Example 3: Projecting Points onto Plane
```csharp
// Create vertical plane (YZ plane at X = 0)
Plane yzPlane = new Plane(Point3d.Origin, Vector3d.XAxis);

// Point to project
Point3d testPoint = new Point3d(10, 5, 3);

// Get closest point on plane (projection)
Point3d projected = yzPlane.GetClosestPointTo(testPoint);

// Calculate projection distance
double distance = yzPlane.DistanceTo(testPoint);

ed.WriteMessage($"\nOriginal point: {testPoint}");
ed.WriteMessage($"\nProjected point: {projected}");
ed.WriteMessage($"\nProjection distance: {distance:F2}");

// Draw line showing projection
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line projLine = new Line(testPoint, projected);
    projLine.ColorIndex = 1; // Red
    
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    btr.AppendEntity(projLine);
    tr.AddNewlyCreatedDBObject(projLine, true);
    tr.Commit();
}
```

### Example 4: Mirroring Across Plane
```csharp
// Create mirror plane (YZ plane)
Plane mirrorPlane = new Plane(Point3d.Origin, Vector3d.XAxis);

// Create mirror transformation
Matrix3d mirror = Matrix3d.Mirroring(mirrorPlane);

using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Select entity to mirror
    Entity ent = tr.GetObject(selectedId, OpenMode.ForWrite) as Entity;
    
    // Apply mirror transformation
    ent.TransformBy(mirror);
    
    tr.Commit();
}

ed.WriteMessage($"\nEntity mirrored across plane");
ed.WriteMessage($"\nPlane normal: {mirrorPlane.Normal}");
ed.WriteMessage($"\nPlane point: {mirrorPlane.PointOnPlane}");
```

### Example 5: Checking Plane Relationships
```csharp
// Create planes
Plane xyPlane = new Plane(Point3d.Origin, Vector3d.ZAxis);
Plane parallelPlane = new Plane(new Point3d(0, 0, 10), Vector3d.ZAxis);
Plane perpPlane = new Plane(Point3d.Origin, Vector3d.XAxis);

// Check relationships
bool isParallel = xyPlane.IsParallelTo(parallelPlane); // true
bool isPerpendicular = xyPlane.IsPerpendicular(perpPlane); // true
bool isCoplanar = xyPlane.IsCoplanarTo(parallelPlane); // false

ed.WriteMessage($"\nXY parallel to offset XY: {isParallel}");
ed.WriteMessage($"\nXY perpendicular to YZ: {isPerpendicular}");
ed.WriteMessage($"\nXY coplanar to offset XY: {isCoplanar}");

// Check if same plane
Plane samePlane = new Plane(new Point3d(5, 5, 0), Vector3d.ZAxis);
bool isCoplanar2 = xyPlane.IsCoplanarTo(samePlane); // true (same plane)

ed.WriteMessage($"\nXY coplanar to same plane: {isCoplanar2}");
```

### Example 6: Creating Plane from Entity
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Get a planar entity (e.g., Circle, Region, Hatch)
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    // Create plane from circle's properties
    Plane circlePlane = new Plane(circle.Center, circle.Normal);
    
    ed.WriteMessage($"\nCircle center: {circle.Center}");
    ed.WriteMessage($"\nCircle normal: {circle.Normal}");
    ed.WriteMessage($"\nPlane created from circle");
    
    // Check if point is on circle's plane
    Point3d testPt = new Point3d(10, 10, 0);
    double dist = circlePlane.DistanceTo(testPt);
    bool isOnPlane = Math.Abs(dist) < 0.001;
    
    ed.WriteMessage($"\nTest point on plane: {isOnPlane}");
    
    tr.Commit();
}
```

### Example 7: Plane from Face Normal
```csharp
// Create plane from three points defining a face
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(5, 10, 5);

// Calculate normal using cross product
Vector3d v1 = p1.GetVectorTo(p2);
Vector3d v2 = p1.GetVectorTo(p3);
Vector3d normal = v1.CrossProduct(v2).GetNormal();

// Create plane
Plane facePlane = new Plane(p1, normal);

// Alternative: use 3-point constructor
Plane facePlane2 = new Plane(p1, p2, p3);

ed.WriteMessage($"\nPlane from vectors - Normal: {facePlane.Normal}");
ed.WriteMessage($"\nPlane from 3 points - Normal: {facePlane2.Normal}");
```

### Example 8: Checking Point Side of Plane
```csharp
// Create horizontal plane at Z = 5
Plane plane = new Plane(new Point3d(0, 0, 5), Vector3d.ZAxis);

Point3d above = new Point3d(0, 0, 10);
Point3d below = new Point3d(0, 0, 2);
Point3d onPlane = new Point3d(5, 5, 5);

// Check which side of plane
double distAbove = plane.DistanceTo(above);
double distBelow = plane.DistanceTo(below);
double distOn = plane.DistanceTo(onPlane);

if (distAbove > 0.001)
    ed.WriteMessage("\nPoint is above plane (in normal direction)");
else if (distAbove < -0.001)
    ed.WriteMessage("\nPoint is below plane");
else
    ed.WriteMessage("\nPoint is on plane");

ed.WriteMessage($"\nDistances: above={distAbove:F2}, below={distBelow:F2}, on={distOn:F6}");
```

## Common Patterns

### Creating Construction Plane
```csharp
// Get UCS plane
Editor ed = Application.DocumentManager.MdiActiveDocument.Editor;
Matrix3d ucs = ed.CurrentUserCoordinateSystem;
CoordinateSystem3d ucsCS = ucs.CoordinateSystem3d;

Plane ucsPlane = new Plane(ucsCS.Origin, ucsCS.Zaxis);
```

### Checking if Points are Coplanar
```csharp
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(10, 0, 0);
Point3d p3 = new Point3d(0, 10, 0);
Point3d p4 = new Point3d(5, 5, 0);

// Create plane from first three points
Plane plane = new Plane(p1, p2, p3);

// Check if fourth point is on plane
double distance = plane.DistanceTo(p4);
bool isCoplanar = Math.Abs(distance) < 0.001;

ed.WriteMessage($"\nPoints are coplanar: {isCoplanar}");
```

### Getting Perpendicular Plane
```csharp
// Original plane (XY)
Plane xyPlane = new Plane(Point3d.Origin, Vector3d.ZAxis);

// Create perpendicular plane through X axis
Plane perpPlane = new Plane(Point3d.Origin, Vector3d.YAxis);

// Verify perpendicularity
bool isPerp = xyPlane.IsPerpendicular(perpPlane);
ed.WriteMessage($"\nPlanes perpendicular: {isPerp}");
```

## Best Practices

1. **Normal Direction**: Ensure normal vector is normalized for consistent results
2. **Three Points**: When creating from 3 points, ensure they're not collinear
3. **Signed Distance**: Remember distance is signed (positive/negative based on normal)
4. **Tolerance**: Use tolerance when checking if points are on plane
5. **Coordinate Systems**: Planes are fundamental for UCS and coordinate transformations
6. **Mirroring**: Use planes for mirror transformations
7. **Projections**: Use `GetClosestPointTo()` for point-to-plane projections

## Related Classes
- **Vector3d** - Normal vector for plane definition
- **Point3d** - Point on plane
- **Matrix3d** - Plane transformations (mirroring, projection)
- **Line3d** - Line-plane intersections
- **CoordinateSystem3d** - Coordinate system definition

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Plane Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Plane)
- [Geometry Namespace](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry)
