# Polyline3d Class

## Overview
The `Polyline3d` class represents a 3D polyline entity in AutoCAD. Similar to `Polyline2d`, it is a container object that owns a collection of `PolylineVertex3d` objects. Each vertex can have a unique 3D position, making it suitable for representing 3D paths, pipelines, and spatial curves.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Polyline3d
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `PolyType` | `Poly3dType` | Gets/sets the polyline type (SimplePoly, QuadSplinePoly, CubicSplinePoly) |
| `Closed` | `bool` | Gets/sets whether the polyline is closed |
| `Length` | `double` | Gets the total length of the polyline |
| `StartPoint` | `Point3d` | Gets the start point of the polyline |
| `EndPoint` | `Point3d` | Gets the end point of the polyline |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetEnumerator()` | `IEnumerator` | Gets an enumerator for vertices |
| `AppendVertex(PolylineVertex3d)` | `void` | Appends a vertex to the polyline |
| `InsertVertexAt(int, PolylineVertex3d)` | `void` | Inserts a vertex at the specified index |
| `OpenVertex(PolylineVertex3d, OpenMode)` | `PolylineVertex3d` | Opens a vertex for read or write |
| `ConvertToPolylineType(Poly3dType)` | `void` | Converts the polyline to a different type |
| `Straighten()` | `void` | Removes all curve fitting |
| `SplineFit()` | `void` | Applies spline fitting |

## Code Examples

### Example 1: Creating a 3D Polyline
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], OpenMode.ForWrite) as BlockTableRecord;
    
    // Create the 3D polyline container
    Polyline3d poly = new Polyline3d();
    poly.SetDatabaseDefaults();
    
    // Add polyline to the drawing
    ObjectId polyId = btr.AppendEntity(poly);
    tr.AddNewlyCreatedDBObject(poly, true);
    
    // Create 3D vertices
    Point3d[] points = new Point3d[]
    {
        new Point3d(0, 0, 0),
        new Point3d(10, 0, 5),
        new Point3d(10, 10, 10),
        new Point3d(0, 10, 15),
        new Point3d(0, 0, 20)
    };
    
    foreach (Point3d pt in points)
    {
        PolylineVertex3d vertex = new PolylineVertex3d(pt);
        poly.AppendVertex(vertex);
        tr.AddNewlyCreatedDBObject(vertex, true);
    }
    
    tr.Commit();
}
```

### Example 2: Reading 3D Polyline Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = tr.GetObject(polylineId, OpenMode.ForRead) as Polyline3d;
    
    ed.WriteMessage($"\n=== Polyline3d Information ===");
    ed.WriteMessage($"\nType: {poly.PolyType}");
    ed.WriteMessage($"\nClosed: {poly.Closed}");
    ed.WriteMessage($"\nLength: {poly.Length:F3}");
    ed.WriteMessage($"\nStart Point: {poly.StartPoint}");
    ed.WriteMessage($"\nEnd Point: {poly.EndPoint}");
    
    ed.WriteMessage("\n\nVertices:");
    int index = 0;
    
    foreach (ObjectId vertexId in poly)
    {
        PolylineVertex3d vertex = tr.GetObject(vertexId, OpenMode.ForRead) as PolylineVertex3d;
        
        Point3d pos = vertex.Position;
        ed.WriteMessage($"\n  Vertex {index}: ({pos.X:F2}, {pos.Y:F2}, {pos.Z:F2})");
        ed.WriteMessage($"\n    Vertex Type: {vertex.VertexType}");
        
        index++;
    }
    
    tr.Commit();
}
```

### Example 3: Creating a Closed 3D Polyline
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline3d poly = new Polyline3d();
    poly.SetDatabaseDefaults();
    
    btr.AppendEntity(poly);
    tr.AddNewlyCreatedDBObject(poly, true);
    
    // Create a 3D helix
    int segments = 20;
    double radius = 5.0;
    double height = 20.0;
    
    for (int i = 0; i <= segments; i++)
    {
        double angle = (i * 2 * Math.PI) / segments;
        double z = (i * height) / segments;
        
        Point3d pt = new Point3d(
            radius * Math.Cos(angle),
            radius * Math.Sin(angle),
            z
        );
        
        PolylineVertex3d vertex = new PolylineVertex3d(pt);
        poly.AppendVertex(vertex);
        tr.AddNewlyCreatedDBObject(vertex, true);
    }
    
    tr.Commit();
}
```

### Example 4: Modifying 3D Polyline Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = tr.GetObject(polylineId, OpenMode.ForRead) as Polyline3d;
    
    int index = 0;
    foreach (ObjectId vertexId in poly)
    {
        PolylineVertex3d vertex = tr.GetObject(vertexId, OpenMode.ForWrite) as PolylineVertex3d;
        
        // Offset each vertex upward by its index
        Point3d currentPos = vertex.Position;
        vertex.Position = new Point3d(currentPos.X, currentPos.Y, currentPos.Z + index);
        
        index++;
    }
    
    tr.Commit();
}
```

### Example 5: Converting to Spline
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = tr.GetObject(polylineId, OpenMode.ForWrite) as Polyline3d;
    
    ed.WriteMessage($"\nBefore: {poly.PolyType}");
    
    // Convert to cubic spline
    poly.ConvertToPolylineType(Poly3dType.CubicSplinePoly);
    
    ed.WriteMessage($"\nAfter: {poly.PolyType}");
    
    tr.Commit();
}
```

### Example 6: Setting Polyline3d Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = tr.GetObject(polylineId, OpenMode.ForWrite) as Polyline3d;
    
    // Set graphical properties
    poly.Layer = "3D-Paths";
    poly.ColorIndex = 5; // Blue
    poly.Linetype = "CONTINUOUS";
    poly.LinetypeScale = 1.0;
    
    // Close the polyline
    poly.Closed = true;
    
    ed.WriteMessage($"\nPolyline closed: {poly.Closed}");
    ed.WriteMessage($"\nNew length: {poly.Length:F3}");
    
    tr.Commit();
}
```

### Example 7: Calculating Polyline Statistics
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Polyline3d poly = tr.GetObject(polylineId, OpenMode.ForRead) as Polyline3d;
    
    int vertexCount = 0;
    double minZ = double.MaxValue;
    double maxZ = double.MinValue;
    Point3d centroid = Point3d.Origin;
    
    foreach (ObjectId vertexId in poly)
    {
        PolylineVertex3d vertex = tr.GetObject(vertexId, OpenMode.ForRead) as PolylineVertex3d;
        Point3d pos = vertex.Position;
        
        vertexCount++;
        minZ = Math.Min(minZ, pos.Z);
        maxZ = Math.Max(maxZ, pos.Z);
        centroid = new Point3d(
            centroid.X + pos.X,
            centroid.Y + pos.Y,
            centroid.Z + pos.Z
        );
    }
    
    if (vertexCount > 0)
    {
        centroid = new Point3d(
            centroid.X / vertexCount,
            centroid.Y / vertexCount,
            centroid.Z / vertexCount
        );
    }
    
    ed.WriteMessage($"\n=== Polyline3d Statistics ===");
    ed.WriteMessage($"\nVertex Count: {vertexCount}");
    ed.WriteMessage($"\nTotal Length: {poly.Length:F3}");
    ed.WriteMessage($"\nMin Z: {minZ:F3}");
    ed.WriteMessage($"\nMax Z: {maxZ:F3}");
    ed.WriteMessage($"\nHeight Range: {(maxZ - minZ):F3}");
    ed.WriteMessage($"\nCentroid: ({centroid.X:F2}, {centroid.Y:F2}, {centroid.Z:F2})");
    
    tr.Commit();
}
```

### Example 8: Creating a 3D Spiral Staircase
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Polyline3d poly = new Polyline3d();
    poly.SetDatabaseDefaults();
    poly.ColorIndex = 1; // Red
    
    btr.AppendEntity(poly);
    tr.AddNewlyCreatedDBObject(poly, true);
    
    // Spiral staircase parameters
    int steps = 30;
    double radius = 10.0;
    double totalHeight = 30.0;
    double rotations = 2.0; // Number of full rotations
    
    for (int i = 0; i <= steps; i++)
    {
        double t = (double)i / steps;
        double angle = t * rotations * 2 * Math.PI;
        double z = t * totalHeight;
        
        Point3d pt = new Point3d(
            radius * Math.Cos(angle),
            radius * Math.Sin(angle),
            z
        );
        
        PolylineVertex3d vertex = new PolylineVertex3d(pt);
        poly.AppendVertex(vertex);
        tr.AddNewlyCreatedDBObject(vertex, true);
    }
    
    ed.WriteMessage($"\nCreated spiral with {steps + 1} vertices");
    ed.WriteMessage($"\nTotal length: {poly.Length:F3}");
    
    tr.Commit();
}
```

## Polyline3d Types

| Type | Description |
|------|-------------|
| `SimplePoly` | Standard 3D polyline with straight segments |
| `QuadSplinePoly` | Quadratic B-spline through vertices |
| `CubicSplinePoly` | Cubic B-spline through vertices |

## PolylineVertex3d Properties

| Property | Type | Description |
|----------|------|-------------|
| `Position` | `Point3d` | 3D vertex location |
| `VertexType` | `Vertex3dType` | Type of vertex (simple, spline control, spline fit) |

## Polyline3d vs Polyline

| Feature | Polyline (Lightweight) | Polyline3d |
|---------|----------------------|------------|
| Dimensions | 2D only | Full 3D |
| Database Objects | Single object | Container + vertex objects |
| Width Support | Yes (per segment) | No |
| Bulge/Arcs | Yes | No |
| Spline Fitting | No | Yes |
| Use Case | 2D drawings | 3D paths, pipes, cables |

## Best Practices

1. **3D Paths**: Use Polyline3d for true 3D spatial paths
2. **Transaction Management**: Add both polyline and all vertices to transaction
3. **Vertex Ownership**: Vertices are owned by polyline and deleted with it
4. **Performance**: For many vertices, consider using a 3D spline instead
5. **Closed Polylines**: Set `Closed = true` to connect endpoints
6. **Spline Fitting**: Use for smooth curves through control points
7. **Z-Coordinates**: Each vertex can have a unique Z value
8. **Visualization**: Set appropriate color and linetype for 3D visibility

## Common Use Cases

- **3D Piping**: Representing pipe runs in 3D space
- **Cable Routing**: Electrical or data cable paths
- **Terrain Contours**: 3D elevation contours
- **Flight Paths**: Aircraft or drone trajectories
- **Helical Structures**: Springs, spirals, DNA strands
- **3D Sketches**: Conceptual 3D line work

## Related Objects
- [Polyline](Polyline.md) - Lightweight 2D polyline
- [Polyline2d](Polyline2d.md) - Legacy 2D polyline
- [Curve](../../BaseClasses/Curve.md) - Base class for polylines
- [Entity](../../BaseClasses/Entity.md) - Base class for all entities
- PolylineVertex3d - Individual 3D vertex object
- [Spline](../Geometric/Spline.md) - Alternative for smooth 3D curves

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Polyline3d Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Polyline3d)
- [Working with 3D Polylines](https://help.autodesk.com/view/OARX/2024/ENU/)
