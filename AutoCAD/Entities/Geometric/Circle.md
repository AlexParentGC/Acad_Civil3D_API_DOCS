# Circle Class

## Overview
The `Circle` class represents a circular entity in AutoCAD, defined by a center point and radius.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Circle
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `Point3d` | Gets/sets the center point of the circle |
| `Radius` | `double` | Gets/sets the radius |
| `Diameter` | `double` | Gets/sets the diameter |
| `Circumference` | `double` | Gets the circumference (read-only) |
| `Area` | `double` | Gets the area (read-only) |
| `Normal` | `Vector3d` | Gets/sets the normal vector (defines plane) |
| `Thickness` | `double` | Gets/sets the thickness (for 3D extrusion) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d, bool)` | `Point3d` | Gets the closest point on the circle |
| `GetPointAtParameter(double)` | `Point3d` | Gets a point at parameter (0-2π) |
| `GetDistAtPoint(Point3d)` | `double` | Gets the distance along the circle to a point |

## Code Examples

### Example 1: Creating a Circle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create a circle at (50, 50, 0) with radius 25
    Circle circle = new Circle();
    circle.Center = new Point3d(50, 50, 0);
    circle.Radius = 25.0;
    
    // Set properties
    circle.Layer = "0";
    circle.ColorIndex = 2; // Yellow
    
    // Add to database
    btr.AppendEntity(circle);
    tr.AddNewlyCreatedDBObject(circle, true);
    
    tr.Commit();
}
```

### Example 2: Getting Circle Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    Point3d center = circle.Center;
    double radius = circle.Radius;
    double diameter = circle.Diameter;
    double circumference = circle.Circumference;
    double area = circle.Area;
    
    ed.WriteMessage($"\nCenter: ({center.X:F2}, {center.Y:F2})");
    ed.WriteMessage($"\nRadius: {radius:F2}");
    ed.WriteMessage($"\nDiameter: {diameter:F2}");
    ed.WriteMessage($"\nCircumference: {circumference:F2}");
    ed.WriteMessage($"\nArea: {area:F2}");
    
    tr.Commit();
}
```

### Example 3: Getting Points on Circle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    // Get points at 0°, 90°, 180°, 270°
    Point3d pt0 = circle.GetPointAtParameter(0); // 0 degrees
    Point3d pt90 = circle.GetPointAtParameter(Math.PI / 2); // 90 degrees
    Point3d pt180 = circle.GetPointAtParameter(Math.PI); // 180 degrees
    Point3d pt270 = circle.GetPointAtParameter(3 * Math.PI / 2); // 270 degrees
    
    tr.Commit();
}
```

### Example 4: Creating Concentric Circles
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d center = new Point3d(100, 100, 0);
    
    // Create 5 concentric circles
    for (int i = 1; i <= 5; i++)
    {
        Circle circle = new Circle();
        circle.Center = center;
        circle.Radius = i * 10.0;
        
        btr.AppendEntity(circle);
        tr.AddNewlyCreatedDBObject(circle, true);
    }
    
    tr.Commit();
}
```

### Example 5: Modifying Circle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForWrite) as Circle;
    
    // Move center
    circle.Center = new Point3d(75, 75, 0);
    
    // Change radius
    circle.Radius = 50.0;
    
    // Or set diameter instead
    circle.Diameter = 100.0; // Same as radius 50
    
    tr.Commit();
}
```

## Common Patterns

### Creating a Circle from 3 Points
```csharp
// Calculate circle from 3 points (requires geometry calculations)
Point3d p1 = new Point3d(0, 0, 0);
Point3d p2 = new Point3d(100, 0, 0);
Point3d p3 = new Point3d(50, 50, 0);

CircularArc3d arc3d = new CircularArc3d(p1, p2, p3);
Point3d center = arc3d.Center;
double radius = arc3d.Radius;

Circle circle = new Circle();
circle.Center = center;
circle.Radius = radius;
```

### Checking if Point is Inside Circle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Circle circle = tr.GetObject(circleId, OpenMode.ForRead) as Circle;
    
    Point3d testPoint = new Point3d(60, 60, 0);
    double distance = circle.Center.DistanceTo(testPoint);
    
    bool isInside = distance <= circle.Radius;
    
    ed.WriteMessage($"\nPoint is {(isInside ? "inside" : "outside")} the circle");
    
    tr.Commit();
}
```

## Related Objects
- [Arc](Arc.md) - Partial circle
- [Ellipse](Ellipse.md) - Oval shape
- [Curve](Curve.md) - Base class
- [Entity](Entity.md) - Base class for all entities

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Circle)
