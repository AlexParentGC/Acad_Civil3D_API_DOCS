# Leader Class

## Overview
The `Leader` class represents a leader annotation entity in AutoCAD. A leader is a line or spline with an arrowhead that connects annotation text or blocks to specific features in a drawing. Leaders are commonly used for callouts, notes, and dimensional annotations.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              └─ Curve
                  └─ Leader
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `HasArrowHead` | `bool` | Gets/sets whether the leader has an arrowhead |
| `HasHookLine` | `bool` | Gets/sets whether the leader has a hook line |
| `IsSplined` | `bool` | Gets/sets whether the leader is splined |
| `Annotation` | `ObjectId` | Gets/sets the annotation object (text, block, etc.) |
| `DimStyle` | `ObjectId` | Gets/sets the dimension style |
| `DimScale` | `double` | Gets/sets the dimension scale |
| `ColorIndex` | `short` | Gets/sets the color index |
| `NumVertices` | `int` | Gets the number of vertices |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `AppendVertex(Point3d)` | `void` | Adds a vertex to the leader |
| `GetVertexAt(int)` | `Point3d` | Gets the vertex at the specified index |
| `SetVertexAt(int, Point3d)` | `void` | Sets the vertex at the specified index |
| `RemoveLastVertex()` | `void` | Removes the last vertex |
| `EvaluateLeader()` | `void` | Recalculates the leader geometry |
| `AttachAnnotation(ObjectId)` | `void` | Attaches an annotation to the leader |

## Code Examples

### Example 1: Creating a Simple Leader
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create leader
    Leader leader = new Leader();
    leader.SetDatabaseDefaults();
    
    // Add vertices (from annotation point to feature)
    leader.AppendVertex(new Point3d(10, 10, 0)); // Annotation point
    leader.AppendVertex(new Point3d(5, 5, 0));   // Elbow point
    leader.AppendVertex(new Point3d(0, 0, 0));   // Feature point (arrowhead)
    
    // Set properties
    leader.HasArrowHead = true;
    leader.ColorIndex = 1; // Red
    
    // Add to drawing
    btr.AppendEntity(leader);
    tr.AddNewlyCreatedDBObject(leader, true);
    
    // Evaluate to update geometry
    leader.EvaluateLeader();
    
    tr.Commit();
}
```

### Example 2: Creating Leader with Text Annotation
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create text annotation
    MText mtext = new MText();
    mtext.SetDatabaseDefaults();
    mtext.Location = new Point3d(10, 10, 0);
    mtext.Contents = "This is a note";
    mtext.TextHeight = 2.5;
    
    ObjectId mtextId = btr.AppendEntity(mtext);
    tr.AddNewlyCreatedDBObject(mtext, true);
    
    // Create leader
    Leader leader = new Leader();
    leader.SetDatabaseDefaults();
    leader.AppendVertex(new Point3d(10, 10, 0));
    leader.AppendVertex(new Point3d(5, 5, 0));
    leader.AppendVertex(new Point3d(0, 0, 0));
    leader.HasArrowHead = true;
    
    // Attach annotation
    leader.Annotation = mtextId;
    
    btr.AppendEntity(leader);
    tr.AddNewlyCreatedDBObject(leader, true);
    leader.EvaluateLeader();
    
    tr.Commit();
}
```

### Example 3: Creating Splined Leader
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Leader leader = new Leader();
    leader.SetDatabaseDefaults();
    
    // Add multiple vertices for smooth curve
    leader.AppendVertex(new Point3d(15, 15, 0));
    leader.AppendVertex(new Point3d(12, 10, 0));
    leader.AppendVertex(new Point3d(8, 8, 0));
    leader.AppendVertex(new Point3d(5, 5, 0));
    leader.AppendVertex(new Point3d(0, 0, 0));
    
    // Make it splined
    leader.IsSplined = true;
    leader.HasArrowHead = true;
    leader.ColorIndex = 3; // Green
    
    btr.AppendEntity(leader);
    tr.AddNewlyCreatedDBObject(leader, true);
    leader.EvaluateLeader();
    
    tr.Commit();
}
```

### Example 4: Reading Leader Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Leader leader = tr.GetObject(leaderId, OpenMode.ForRead) as Leader;
    
    ed.WriteMessage("\n=== Leader Information ===");
    ed.WriteMessage($"\nHas Arrowhead: {leader.HasArrowHead}");
    ed.WriteMessage($"\nHas Hook Line: {leader.HasHookLine}");
    ed.WriteMessage($"\nIs Splined: {leader.IsSplined}");
    ed.WriteMessage($"\nNumber of Vertices: {leader.NumVertices}");
    ed.WriteMessage($"\nAnnotation: {leader.Annotation}");
    
    ed.WriteMessage("\n\nVertices:");
    for (int i = 0; i < leader.NumVertices; i++)
    {
        Point3d vertex = leader.GetVertexAt(i);
        ed.WriteMessage($"\n  Vertex {i}: ({vertex.X:F2}, {vertex.Y:F2}, {vertex.Z:F2})");
    }
    
    tr.Commit();
}
```

### Example 5: Modifying Leader Vertices
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Leader leader = tr.GetObject(leaderId, OpenMode.ForWrite) as Leader;
    
    // Move the middle vertex
    if (leader.NumVertices >= 3)
    {
        Point3d middleVertex = leader.GetVertexAt(1);
        Point3d newVertex = new Point3d(middleVertex.X + 5, middleVertex.Y + 5, middleVertex.Z);
        leader.SetVertexAt(1, newVertex);
        
        leader.EvaluateLeader();
    }
    
    tr.Commit();
}
```

### Example 6: Leader with Block Annotation
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    // Create block reference (assuming "DetailCallout" block exists)
    if (bt.Has("DetailCallout"))
    {
        BlockReference blockRef = new BlockReference(new Point3d(10, 10, 0), bt["DetailCallout"]);
        blockRef.SetDatabaseDefaults();
        
        ObjectId blockRefId = btr.AppendEntity(blockRef);
        tr.AddNewlyCreatedDBObject(blockRef, true);
        
        // Create leader
        Leader leader = new Leader();
        leader.SetDatabaseDefaults();
        leader.AppendVertex(new Point3d(10, 10, 0));
        leader.AppendVertex(new Point3d(5, 5, 0));
        leader.AppendVertex(new Point3d(0, 0, 0));
        leader.HasArrowHead = true;
        leader.Annotation = blockRefId;
        
        btr.AppendEntity(leader);
        tr.AddNewlyCreatedDBObject(leader, true);
        leader.EvaluateLeader();
    }
    
    tr.Commit();
}
```

### Example 7: Setting Leader Dimension Style
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Leader leader = tr.GetObject(leaderId, OpenMode.ForWrite) as Leader;
    
    // Get dimension style table
    DimStyleTable dst = tr.GetObject(db.DimStyleTableId, OpenMode.ForRead) as DimStyleTable;
    
    if (dst.Has("Annotative"))
    {
        leader.DimStyle = dst["Annotative"];
        leader.EvaluateLeader();
        
        ed.WriteMessage("\nApplied 'Annotative' dimension style");
    }
    
    tr.Commit();
}
```

### Example 8: Creating Multiple Leaders
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    
    Point3d centerPoint = new Point3d(0, 0, 0);
    int numLeaders = 8;
    double radius = 10.0;
    
    for (int i = 0; i < numLeaders; i++)
    {
        double angle = (i * 2 * Math.PI) / numLeaders;
        Point3d endPoint = new Point3d(
            centerPoint.X + radius * Math.Cos(angle),
            centerPoint.Y + radius * Math.Sin(angle),
            0
        );
        
        Leader leader = new Leader();
        leader.SetDatabaseDefaults();
        leader.AppendVertex(endPoint);
        leader.AppendVertex(new Point3d(
            centerPoint.X + (radius * 0.5) * Math.Cos(angle),
            centerPoint.Y + (radius * 0.5) * Math.Sin(angle),
            0
        ));
        leader.AppendVertex(centerPoint);
        leader.HasArrowHead = true;
        leader.ColorIndex = (short)(i % 7 + 1);
        
        btr.AppendEntity(leader);
        tr.AddNewlyCreatedDBObject(leader, true);
        leader.EvaluateLeader();
    }
    
    ed.WriteMessage($"\nCreated {numLeaders} leaders");
    
    tr.Commit();
}
```

## Best Practices

1. **Call EvaluateLeader()**: Always call after creating or modifying leader
2. **Vertex Order**: First vertex is annotation point, last is arrowhead point
3. **Minimum Vertices**: Need at least 2 vertices for a valid leader
4. **Annotation Attachment**: Attach annotation for automatic positioning
5. **Dimension Styles**: Use dimension styles for consistent appearance
6. **Splined Leaders**: Use for curved, organic-looking leaders
7. **Hook Lines**: Enable for horizontal landing line at annotation
8. **Color and Linetype**: Set explicitly for visibility

## Leader vs MLeader

| Feature | Leader | MLeader |
|---------|--------|---------|
| Introduction | Legacy (R13+) | Modern (2008+) |
| Flexibility | Basic | Advanced |
| Multiple Leaders | No | Yes |
| Content Types | Limited | Text, Block, None |
| Styles | Dimension Style | MLeader Style |
| Recommended | Legacy compatibility | New drawings |

## Common Use Cases

- **Callouts**: Identifying features in drawings
- **Notes**: Adding explanatory text
- **Detail References**: Pointing to detail views
- **Dimension Annotations**: Supplemental dimensional information
- **Part Labels**: Identifying components

## Related Objects
- [MLeader](MLeader.md) - Modern multi-leader annotation
- [Dimension](Dimension.md) - Dimension annotation base class
- [Curve](../../BaseClasses/Curve.md) - Base class for Leader
- [Entity](../../BaseClasses/Entity.md) - Base class for all entities
- MText - Text annotation for leaders
- BlockReference - Block annotation for leaders

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Leader Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Leader)
