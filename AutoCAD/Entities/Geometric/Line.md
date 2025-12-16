# Line Class

## Overview
The `Line` class represents a straight line segment in AutoCAD. It is one of the most fundamental geometric entities, defined by two endpoints.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Line
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `StartPoint` | `Point3d` | Gets/sets the start point of the line |
| `EndPoint` | `Point3d` | Gets/sets the end point of the line |
| `Delta` | `Vector3d` | Gets the vector from start to end point |
| `Length` | `double` | Gets the length of the line |
| `Angle` | `double` | Gets the angle of the line in radians |
| `Normal` | `Vector3d` | Gets/sets the normal vector |
| `Thickness` | `double` | Gets/sets the thickness (for 3D extrusion) |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetClosestPointTo(Point3d, bool)` | `Point3d` | Gets the closest point on the line to a given point |
| `GetDistAtPoint(Point3d)` | `double` | Gets the distance along the line to a point |
| `GetPointAtDist(double)` | `Point3d` | Gets a point at a specified distance along the line |
| `GetPointAtParameter(double)` | `Point3d` | Gets a point at a parameter value (0-1) |

## Code Examples

### Example 1: Creating a Line
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForWrite) as BlockTableRecord;
    
    // Create a line from (0,0,0) to (100,100,0)
    Line line = new Line(new Point3d(0, 0, 0), new Point3d(100, 100, 0));
    
    // Set properties
    line.Layer = "0";
    line.ColorIndex = 1; // Red
    
    // Add to database
    btr.AppendEntity(line);
    tr.AddNewlyCreatedDBObject(line, true);
    
    tr.Commit();
}
```

### Example 2: Modifying Line Endpoints
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForWrite) as Line;
    
    // Move the start point
    line.StartPoint = new Point3d(10, 10, 0);
    
    // Move the end point
    line.EndPoint = new Point3d(200, 150, 0);
    
    tr.Commit();
}
```

### Example 3: Getting Line Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForRead) as Line;
    
    double length = line.Length;
    double angle = line.Angle * (180.0 / Math.PI); // Convert to degrees
    Vector3d direction = line.Delta.GetNormal(); // Normalized direction vector
    
    Point3d midPoint = line.GetPointAtParameter(0.5); // Midpoint
    
    ed.WriteMessage($"\nLine Length: {length:F2}");
    ed.WriteMessage($"\nLine Angle: {angle:F2}°");
    ed.WriteMessage($"\nMidpoint: ({midPoint.X:F2}, {midPoint.Y:F2})");
    
    tr.Commit();
}
```

### Example 4: Finding Closest Point on Line
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForRead) as Line;
    
    Point3d testPoint = new Point3d(50, 75, 0);
    
    // Get closest point on line (extend = false means don't extend beyond endpoints)
    Point3d closestPoint = line.GetClosestPointTo(testPoint, false);
    
    double distance = testPoint.DistanceTo(closestPoint);
    
    ed.WriteMessage($"\nClosest point: ({closestPoint.X:F2}, {closestPoint.Y:F2})");
    ed.WriteMessage($"\nDistance: {distance:F2}");
    
    tr.Commit();
}
```

### Example 5: Creating Multiple Lines (Rectangle)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d p1 = new Point3d(0, 0, 0);
    Point3d p2 = new Point3d(100, 0, 0);
    Point3d p3 = new Point3d(100, 50, 0);
    Point3d p4 = new Point3d(0, 50, 0);
    
    // Create four lines forming a rectangle
    Line[] lines = new Line[]
    {
        new Line(p1, p2),
        new Line(p2, p3),
        new Line(p3, p4),
        new Line(p4, p1)
    };
    
    foreach (Line line in lines)
    {
        btr.AppendEntity(line);
        tr.AddNewlyCreatedDBObject(line, true);
    }
    
    tr.Commit();
}
```

## Common Patterns

### Creating a Line at an Angle
```csharp
Point3d startPoint = new Point3d(0, 0, 0);
double length = 100.0;
double angleInDegrees = 45.0;
double angleInRadians = angleInDegrees * (Math.PI / 180.0);

Point3d endPoint = new Point3d(
    startPoint.X + length * Math.Cos(angleInRadians),
    startPoint.Y + length * Math.Sin(angleInRadians),
    startPoint.Z
);

Line line = new Line(startPoint, endPoint);
```

### Extending a Line
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Line line = tr.GetObject(lineId, OpenMode.ForWrite) as Line;
    
    // Extend by 10 units in the direction of the line
    Vector3d direction = line.Delta.GetNormal();
    line.EndPoint = line.EndPoint + (direction * 10.0);
    
    tr.Commit();
}
```

## Related Objects
- [Curve](Curve.md) - Base class for Line
- [Entity](Entity.md) - Base class for all entities
- [Polyline](Polyline.md) - Multiple connected line segments
- [Arc](Arc.md) - Curved counterpart to Line

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Line)
