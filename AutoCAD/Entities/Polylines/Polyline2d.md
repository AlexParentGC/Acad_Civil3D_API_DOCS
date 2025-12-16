# Polyline2d Class

## Overview
The `Polyline2d` class represents a legacy 2D polyline entity in AutoCAD. Unlike the lightweight `Polyline` class, `Polyline2d` is a container object that owns a collection of `Vertex2d` objects. Each vertex is a separate database object. This class is primarily used for compatibility with older drawings and for polylines that require per-vertex extended data.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Polyline2d
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `PolyType` | `Poly2dType` | Gets/sets the polyline type (SimplePoly, FitCurvePoly, QuadSplinePoly, CubicSplinePoly) |
| `DefaultStartWidth` | `double` | Gets/sets the default starting width for vertices |
| `DefaultEndWidth` | `double` | Gets/sets the default ending width for vertices |
| `Thickness` | `double` | Gets/sets the polyline thickness (extrusion) |
| `Elevation` | `double` | Gets/sets the elevation (Z coordinate) |
| `Normal` | `Vector3d` | Gets/sets the normal vector |
| `Closed` | `bool` | Gets/sets whether the polyline is closed |
| `LinetypeGenerationOn` | `bool` | Gets/sets whether linetype generation is continuous |
| `Length` | `double` | Gets the total length of the polyline |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetEnumerator()` | `IEnumerator` | Gets an enumerator for vertices |
| `AppendVertex(Vertex2d)` | `void` | Appends a vertex to the polyline |
| `InsertVertexAt(int, Vertex2d)` | `void` | Inserts a vertex at the specified index |
| `OpenVertex(Vertex2d, OpenMode)` | `Vertex2d` | Opens a vertex for read or write |
| `ConvertToPolylineType(Poly2dType)` | `void` | Converts the polyline to a different type |
| `Straighten()` | `void` | Removes all curve fitting |
| `SplineFit()` | `void` | Applies spline fitting |

## Code Examples

### Example 1: Creating a Polyline2d
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], OpenMode.ForWrite) as BlockTableRecord;
    
    // Create the polyline container
    Polyline2d poly = new Polyline2d();
    poly.SetDatabaseDefaults();
    
    // Add polyline to the drawing
    ObjectId polyId = btr.AppendEntity(poly);
    tr.AddNewlyCreatedDBObject(poly, true);
    
    // Create vertices
    Point3d[] points = new Point3d[]
    {
        new Point3d(0, 0, 0),
        new Point3d(10, 0, 0),
        new Point3d(10, 10, 0),
        new Point3d(0, 10, 0)
    };
    
    foreach (Point3d pt in points)
    {
        Vertex2d vertex = new Vertex2d(pt, 0, 0, 0, 0);
        poly.AppendVertex(vertex);
        tr.AddNewlyCreatedDBObject(vertex, true);
    }
    
    // Close the polyline
    poly.Closed = true;
    
    tr.Commit();
}
```

### Example 2: Reading Polyline2d Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline2d poly = tr.GetObject(polylineId, OpenMode.ForRead) as Polyline2d;
    
    ed.WriteMessage($"\n=== Polyline2d Information ===");
    ed.WriteMessage($"\nType: {poly.PolyType}");
    ed.WriteMessage($"\nClosed: {poly.Closed}");
    ed.WriteMessage($"\nLength: {poly.Length:F3}");
    ed.WriteMessage($"\nElevation: {poly.Elevation:F3}");
    
    ed.WriteMessage("\n\nVertices:");
    int index = 0;
    
    foreach (ObjectId vertexId in poly)
    {
        Vertex2d vertex = tr.GetObject(vertexId, OpenMode.ForRead) as Vertex2d;
        
        ed.WriteMessage($"\n  Vertex {index}: ({vertex.Position.X:F2}, {vertex.Position.Y:F2})");
        ed.WriteMessage($"\n    Start Width: {vertex.StartWidth:F2}");
        ed.WriteMessage($"\n    End Width: {vertex.EndWidth:F2}");
        ed.WriteMessage($"\n    Bulge: {vertex.Bulge:F3}");
        
        index++;
    }
    
    tr.Commit();
}
```

### Example 3: Modifying Polyline2d Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline2d poly = tr.GetObject(polylineId, OpenMode.ForRead) as Polyline2d;
    
    int index = 0;
    foreach (ObjectId vertexId in poly)
    {
        Vertex2d vertex = tr.GetObject(vertexId, OpenMode.ForWrite) as Vertex2d;
        
        // Set varying widths
        vertex.StartWidth = index * 0.5;
        vertex.EndWidth = (index + 1) * 0.5;
        
        index++;
    }
    
    tr.Commit();
}
```

### Example 4: Creating a Polyline2d with Bulges (Arcs)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline2d poly = new Polyline2d();
    poly.SetDatabaseDefaults();
    
    btr.AppendEntity(poly);
    tr.AddNewlyCreatedDBObject(poly, true);
    
    // Create vertices with bulges for curved segments
    Point3d[] points = new Point3d[]
    {
        new Point3d(0, 0, 0),
        new Point3d(10, 0, 0),
        new Point3d(10, 10, 0),
        new Point3d(0, 10, 0)
    };
    
    double[] bulges = { 0.5, 0, -0.5, 0 }; // Positive = counterclockwise arc
    
    for (int i = 0; i < points.Length; i++)
    {
        Vertex2d vertex = new Vertex2d(points[i], 0, 0, 0, 0);
        vertex.Bulge = bulges[i];
        poly.AppendVertex(vertex);
        tr.AddNewlyCreatedDBObject(vertex, true);
    }
    
    poly.Closed = true;
    
    tr.Commit();
}
```

### Example 5: Converting Polyline2d to Spline
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline2d poly = tr.GetObject(polylineId, OpenMode.ForWrite) as Polyline2d;
    
    // Convert to cubic spline
    poly.ConvertToPolylineType(Poly2dType.CubicSplinePoly);
    
    ed.WriteMessage($"\nConverted to: {poly.PolyType}");
    
    tr.Commit();
}
```

### Example 6: Setting Polyline2d Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline2d poly = tr.GetObject(polylineId, OpenMode.ForWrite) as Polyline2d;
    
    // Set properties
    poly.Layer = "Polylines";
    poly.ColorIndex = 3; // Green
    poly.Linetype = "DASHED";
    poly.LinetypeScale = 2.0;
    poly.LinetypeGenerationOn = true; // Continuous linetype through vertices
    
    // Set default widths
    poly.DefaultStartWidth = 0.5;
    poly.DefaultEndWidth = 0.5;
    
    // Set elevation and thickness
    poly.Elevation = 10.0;
    poly.Thickness = 2.0;
    
    tr.Commit();
}
```

### Example 7: Inserting Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline2d poly = tr.GetObject(polylineId, OpenMode.ForWrite) as Polyline2d;
    
    // Insert a new vertex at index 1
    Vertex2d newVertex = new Vertex2d(new Point3d(5, 5, 0), 0, 0, 0, 0);
    newVertex.StartWidth = 0.25;
    newVertex.EndWidth = 0.25;
    
    poly.InsertVertexAt(1, newVertex);
    tr.AddNewlyCreatedDBObject(newVertex, true);
    
    ed.WriteMessage("\nInserted vertex at index 1");
    
    tr.Commit();
}
```

### Example 8: Straightening a Polyline2d
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline2d poly = tr.GetObject(polylineId, OpenMode.ForWrite) as Polyline2d;
    
    ed.WriteMessage($"\nBefore straighten - Type: {poly.PolyType}");
    
    // Remove all curve fitting and bulges
    poly.Straighten();
    
    ed.WriteMessage($"\nAfter straighten - Type: {poly.PolyType}");
    
    tr.Commit();
}
```

## Polyline2d Types

| Type | Description |
|------|-------------|
| `SimplePoly` | Standard polyline with straight and arc segments |
| `FitCurvePoly` | Curve-fit polyline through vertices |
| `QuadSplinePoly` | Quadratic B-spline |
| `CubicSplinePoly` | Cubic B-spline |

## Vertex2d Properties

| Property | Type | Description |
|----------|------|-------------|
| `Position` | `Point3d` | Vertex location |
| `StartWidth` | `double` | Starting width at this vertex |
| `EndWidth` | `double` | Ending width at this vertex |
| `Bulge` | `double` | Arc bulge factor (0 = straight, ±1 = semicircle) |
| `TangentUsed` | `bool` | Whether tangent direction is used |
| `Tangent` | `double` | Tangent direction angle |

## Polyline vs Polyline2d

| Feature | Polyline (Lightweight) | Polyline2d (Legacy) |
|---------|----------------------|---------------------|
| Database Objects | Single object | Container + vertex objects |
| Memory Usage | Lower | Higher |
| Performance | Faster | Slower |
| Per-Vertex XData | No | Yes (each vertex is a DBObject) |
| 3D Support | No | No (use Polyline3d) |
| Recommended | Yes (modern) | Only for compatibility |

## Best Practices

1. **Use Lightweight Polyline**: Prefer `Polyline` over `Polyline2d` for new drawings
2. **Transaction Management**: Remember to add both polyline and vertices to transaction
3. **Vertex Ownership**: Vertices are owned by the polyline and deleted with it
4. **Bulge Calculation**: Bulge = tan(angle/4), where angle is the included angle
5. **Linetype Generation**: Set `LinetypeGenerationOn = true` for continuous linetypes
6. **Elevation**: All vertices share the same elevation (Z coordinate)
7. **Closed Polylines**: Set `Closed = true` to connect last vertex to first
8. **Width Variation**: Use per-vertex widths for tapered segments

## Common Use Cases

- **Legacy Drawing Compatibility**: Reading/writing older DWG files
- **Per-Vertex Extended Data**: When you need XData on individual vertices
- **Curve Fitting**: Spline-fit polylines through control points
- **Variable Width Polylines**: Tapered line segments

## Related Objects
- [Polyline](Polyline.md) - Modern lightweight 2D polyline
- [Polyline3d](Polyline3d.md) - 3D polyline
- [Curve](../../BaseClasses/Curve.md) - Base class for polylines
- [Entity](../../BaseClasses/Entity.md) - Base class for all entities
- Vertex2d - Individual vertex object

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Polyline2d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Polyline2d)
- [Working with Polylines](https://help.autodesk.com/view/OARX/2024/ENU/)
