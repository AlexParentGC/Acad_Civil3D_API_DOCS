# Extents3d Struct

## Overview
The `Extents3d` struct represents a 3D bounding box defined by minimum and maximum points. It is used for calculating entity bounds, zoom extents, and spatial queries.

## Namespace
`Autodesk.AutoCAD.Geometry`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `MinPoint` | `Point3d` | Gets/sets the minimum corner point |
| `MaxPoint` | `Point3d` | Gets/sets the maximum corner point |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `AddPoint(Point3d)` | `void` | Expands extents to include point |
| `AddExtents(Extents3d)` | `void` | Expands extents to include another extents |
| `ExpandBy(Vector3d)` | `void` | Expands extents by vector offset |
| `TransformBy(Matrix3d)` | `void` | Transforms extents by matrix |
| `IsEqualTo(Extents3d)` | `bool` | Checks equality |

## Code Examples

### Example 1: Getting Entity Extents
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Get entity's bounding box
    Extents3d extents = ent.GeometricExtents;
    
    Point3d min = extents.MinPoint;
    Point3d max = extents.MaxPoint;
    
    // Calculate dimensions
    double width = max.X - min.X;
    double height = max.Y - min.Y;
    double depth = max.Z - min.Z;
    
    ed.WriteMessage($"\nMin: ({min.X:F2}, {min.Y:F2}, {min.Z:F2})");
    ed.WriteMessage($"\nMax: ({max.X:F2}, {max.Y:F2}, {max.Z:F2})");
    ed.WriteMessage($"\nDimensions: {width:F2} x {height:F2} x {depth:F2}");
    
    tr.Commit();
}
```

### Example 2: Combining Multiple Entity Extents
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    Extents3d? totalExtents = null;
    
    foreach (ObjectId id in btr)
    {
        Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
        if (ent == null) continue;
        
        try
        {
            Extents3d entExtents = ent.GeometricExtents;
            
            if (totalExtents == null)
                totalExtents = entExtents;
            else
                totalExtents.Value.AddExtents(entExtents);
        }
        catch { }
    }
    
    if (totalExtents != null)
    {
        ed.WriteMessage($"\nTotal extents:");
        ed.WriteMessage($"\n  Min: {totalExtents.Value.MinPoint}");
        ed.WriteMessage($"\n  Max: {totalExtents.Value.MaxPoint}");
    }
    
    tr.Commit();
}
```

### Example 3: Expanding Extents
```csharp
Point3d min = new Point3d(0, 0, 0);
Point3d max = new Point3d(10, 10, 0);
Extents3d extents = new Extents3d(min, max);

// Add a point (expands if outside current bounds)
Point3d newPoint = new Point3d(15, 5, 0);
extents.AddPoint(newPoint);

ed.WriteMessage($"\nExpanded max: {extents.MaxPoint}"); // (15, 10, 0)

// Expand by offset
Vector3d offset = new Vector3d(1, 1, 1);
extents.ExpandBy(offset);

ed.WriteMessage($"\nAfter expand: Min={extents.MinPoint}, Max={extents.MaxPoint}");
```

### Example 4: Zoom to Extents
```csharp
[CommandMethod("ZOOMEXTENTS")]
public void ZoomToExtents()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
        
        Extents3d? extents = null;
        
        foreach (ObjectId id in btr)
        {
            Entity ent = tr.GetObject(id, OpenMode.ForRead) as Entity;
            if (ent == null) continue;
            
            try
            {
                if (extents == null)
                    extents = ent.GeometricExtents;
                else
                    extents.Value.AddExtents(ent.GeometricExtents);
            }
            catch { }
        }
        
        if (extents != null)
        {
            // Zoom to extents
            Point3d min = extents.Value.MinPoint;
            Point3d max = extents.Value.MaxPoint;
            
            ViewTableRecord view = ed.GetCurrentView();
            view.CenterPoint = new Point2d(
                (min.X + max.X) / 2,
                (min.Y + max.Y) / 2
            );
            
            view.Height = max.Y - min.Y;
            view.Width = max.X - min.X;
            
            ed.SetCurrentView(view);
        }
        
        tr.Commit();
    }
}
```

## Best Practices

1. **Null Checks**: Check for null when getting entity extents
2. **Try-Catch**: Some entities may not have valid extents
3. **Combining**: Use `AddExtents()` to combine multiple bounding boxes
4. **Margins**: Use `ExpandBy()` to add margins around extents

## Related Classes
- **Point3d** - Min/max corner points
- **Vector3d** - For expanding extents
- **Entity** - All entities have `GeometricExtents` property
- **Extents2d** - 2D bounding box

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [Extents3d Struct Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_Geometry_Extents3d)
