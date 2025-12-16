# Polyline Class

## Overview
The `Polyline` class represents a lightweight 2D polyline in AutoCAD. It's an optimized version that uses less memory than the older Polyline2d class.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Polyline
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `NumberOfVertices` | `int` | Gets the number of vertices |
| `Closed` | `bool` | Gets/sets whether the polyline is closed |
| `Length` | `double` | Gets the total length |
| `Area` | `double` | Gets the area (if closed) |
| `Elevation` | `double` | Gets/sets the elevation (Z value) |
| `Normal` | `Vector3d` | Gets/sets the normal vector |
| `Thickness` | `double` | Gets/sets the thickness |
| `ConstantWidth` | `double` | Gets/sets constant width for all segments |
| `HasWidth` | `bool` | Gets whether polyline has width |
| `HasBulges` | `bool` | Gets whether polyline has bulges (arcs) |
| `Plinegen` | `bool` | Gets/sets linetype generation mode |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `AddVertexAt(int, Point2d, double, double, double)` | `void` | Adds a vertex at specified index |
| `RemoveVertexAt(int)` | `void` | Removes vertex at index |
| `GetPoint2dAt(int)` | `Point2d` | Gets 2D point at vertex index |
| `GetPoint3dAt(int)` | `Point3d` | Gets 3D point at vertex index |
| `SetPointAt(int, Point2d)` | `void` | Sets point at vertex index |
| `GetBulgeAt(int)` | `double` | Gets bulge value at vertex |
| `SetBulgeAt(int, double)` | `void` | Sets bulge value at vertex |
| `GetStartWidthAt(int)` | `double` | Gets start width at segment |
| `GetEndWidthAt(int)` | `double` | Gets end width at segment |
| `SetWidthsAt(int, double, double)` | `void` | Sets start/end widths at segment |
| `GetSegmentType(int)` | `SegmentType` | Gets segment type (Line or Arc) |

## Code Examples

### Example 1: Creating a Simple Polyline
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline pline = new Polyline();
    
    // Add vertices
    pline.AddVertexAt(0, new Point2d(0, 0), 0, 0, 0);
    pline.AddVertexAt(1, new Point2d(100, 0), 0, 0, 0);
    pline.AddVertexAt(2, new Point2d(100, 50), 0, 0, 0);
    pline.AddVertexAt(3, new Point2d(0, 50), 0, 0, 0);
    
    // Close the polyline
    pline.Closed = true;
    
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}
```

### Example 2: Creating Polyline with Bulges (Arcs)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline pline = new Polyline();
    
    // Add vertices with bulges
    // Bulge = tan(angle/4), where angle is the included angle
    // Bulge of 1 = 180° semicircle
    pline.AddVertexAt(0, new Point2d(0, 0), 0, 0, 0); // Straight segment
    pline.AddVertexAt(1, new Point2d(100, 0), 1, 0, 0); // Arc segment (bulge=1)
    pline.AddVertexAt(2, new Point2d(100, 100), 0, 0, 0); // Straight segment
    
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}
```

### Example 3: Creating Polyline with Width
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline pline = new Polyline();
    
    // Add vertices with varying widths
    pline.AddVertexAt(0, new Point2d(0, 0), 0, 5, 5); // Width 5
    pline.AddVertexAt(1, new Point2d(100, 0), 0, 5, 10); // Taper from 5 to 10
    pline.AddVertexAt(2, new Point2d(100, 100), 0, 10, 10); // Width 10
    
    btr.AppendEntity(pline);
    tr.AddNewlyCreatedDBObject(pline, true);
    
    tr.Commit();
}
```

### Example 4: Iterating Through Polyline Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = tr.GetObject(plineId, OpenMode.ForRead) as Polyline;
    
    int numVertices = pline.NumberOfVertices;
    
    for (int i = 0; i < numVertices; i++)
    {
        Point2d pt2d = pline.GetPoint2dAt(i);
        Point3d pt3d = pline.GetPoint3dAt(i);
        double bulge = pline.GetBulgeAt(i);
        SegmentType segType = pline.GetSegmentType(i);
        
        ed.WriteMessage($"\nVertex {i}: ({pt2d.X:F2}, {pt2d.Y:F2})");
        ed.WriteMessage($"\n  Bulge: {bulge:F4}");
        ed.WriteMessage($"\n  Segment Type: {segType}");
    }
    
    tr.Commit();
}
```

### Example 5: Modifying Polyline Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = tr.GetObject(plineId, OpenMode.ForWrite) as Polyline;
    
    // Move a vertex
    pline.SetPointAt(1, new Point2d(150, 50));
    
    // Add a new vertex
    pline.AddVertexAt(2, new Point2d(125, 75), 0, 0, 0);
    
    // Remove a vertex
    pline.RemoveVertexAt(0);
    
    // Set bulge for arc segment
    pline.SetBulgeAt(1, 0.5);
    
    tr.Commit();
}
```

### Example 6: Getting Polyline Area and Length
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline pline = tr.GetObject(plineId, OpenMode.ForRead) as Polyline;
    
    double length = pline.Length;
    double area = pline.Closed ? pline.Area : 0;
    
    ed.WriteMessage($"\nPolyline Length: {length:F2}");
    if (pline.Closed)
    {
        ed.WriteMessage($"\nPolyline Area: {area:F2}");
    }
    
    tr.Commit();
}
```

## Common Patterns

### Creating a Rectangle
```csharp
Polyline rect = new Polyline();
rect.AddVertexAt(0, new Point2d(0, 0), 0, 0, 0);
rect.AddVertexAt(1, new Point2d(100, 0), 0, 0, 0);
rect.AddVertexAt(2, new Point2d(100, 50), 0, 0, 0);
rect.AddVertexAt(3, new Point2d(0, 50), 0, 0, 0);
rect.Closed = true;
```

### Converting Line Segments to Polyline
```csharp
// Assuming you have connected line segments
Polyline pline = new Polyline();
int index = 0;
foreach (Line line in lines)
{
    Point2d pt = new Point2d(line.StartPoint.X, line.StartPoint.Y);
    pline.AddVertexAt(index++, pt, 0, 0, 0);
}
// Add last endpoint
Point2d lastPt = new Point2d(lines.Last().EndPoint.X, lines.Last().EndPoint.Y);
pline.AddVertexAt(index, lastPt, 0, 0, 0);
```

## Related Objects
- [Polyline2d](Polyline2d.md) - Older 2D polyline format
- [Polyline3d](Polyline3d.md) - 3D polyline
- [Line](Line.md) - Single line segment
- [Curve](Curve.md) - Base class

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Polyline)
