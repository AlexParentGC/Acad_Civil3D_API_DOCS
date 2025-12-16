# Entity Class

## Overview
The `Entity` class is the base class for all graphical objects in AutoCAD. It inherits from `DBObject` and provides common properties and methods for all drawable entities like lines, circles, text, etc.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ Entity
              ├─ Curve (Line, Arc, Circle, Polyline, etc.)
              ├─ BlockReference
              ├─ DBText
              ├─ MText
              ├─ Dimension
              ├─ Hatch
              └─ ... (all graphical entities)
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Layer` | `string` | Gets/sets the layer name |
| `LayerId` | `ObjectId` | Gets/sets the layer ObjectId |
| `Color` | `Color` | Gets/sets the entity color |
| `ColorIndex` | `short` | Gets/sets the color index (0-256) |
| `Linetype` | `string` | Gets/sets the linetype name |
| `LinetypeId` | `ObjectId` | Gets/sets the linetype ObjectId |
| `LinetypeScale` | `double` | Gets/sets the linetype scale |
| `Lineweight` | `LineWeight` | Gets/sets the lineweight |
| `Visible` | `bool` | Gets/sets visibility |
| `Transparency` | `Transparency` | Gets/sets transparency |
| `Material` | `string` | Gets/sets the material name |
| `MaterialId` | `ObjectId` | Gets/sets the material ObjectId |
| `PlotStyleName` | `string` | Gets/sets the plot style name |
| `Bounds` | `Extents3d?` | Gets the bounding box extents |
| `GeometricExtents` | `Extents3d` | Gets the geometric extents |
| `BlockId` | `ObjectId` | Gets the ObjectId of the owning BlockTableRecord |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetBoundingBox()` | `void` | Gets the bounding box (out parameters) |
| `GetGeometricExtents()` | `void` | Gets geometric extents (out parameters) |
| `Highlight()` | `void` | Highlights the entity |
| `Unhighlight()` | `void` | Removes highlighting |
| `GetGripPoints()` | `void` | Gets grip points for the entity |
| `MoveGripPointsAt()` | `void` | Moves grip points |
| `GetStretchPoints()` | `void` | Gets stretch points |
| `MoveStretchPointsAt()` | `void` | Moves stretch points |
| `GetOsnapPoints()` | `void` | Gets object snap points |
| `Intersect()` | `void` | Finds intersection points with another entity |
| `GetTransformedCopy()` | `Entity` | Gets a transformed copy of the entity |
| `TransformBy()` | `void` | Transforms the entity by a matrix |
| `Draw()` | `void` | Draws the entity |

## Code Examples

### Example 1: Changing Entity Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Change layer
    ent.Layer = "NewLayer";
    
    // Change color to red
    ent.ColorIndex = 1;
    
    // Change linetype
    ent.Linetype = "DASHED";
    
    // Change linetype scale
    ent.LinetypeScale = 2.0;
    
    // Change lineweight
    ent.Lineweight = LineWeight.LineWeight050;
    
    // Set transparency (0-255, where 0 is opaque)
    ent.Transparency = new Transparency(128);
    
    tr.Commit();
}
```

### Example 2: Getting Entity Bounds
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Get geometric extents
    Extents3d extents = ent.GeometricExtents;
    
    Point3d minPoint = extents.MinPoint;
    Point3d maxPoint = extents.MaxPoint;
    
    double width = maxPoint.X - minPoint.X;
    double height = maxPoint.Y - minPoint.Y;
    
    ed.WriteMessage($"\nEntity bounds: {width} x {height}");
    
    tr.Commit();
}
```

### Example 3: Highlighting Entities
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForRead) as Entity;
    
    // Highlight the entity
    ent.Highlight();
    
    // Wait for user input or delay
    System.Threading.Thread.Sleep(2000);
    
    // Remove highlight
    ent.Unhighlight();
    
    tr.Commit();
}
```

### Example 4: Transforming Entities
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Create a transformation matrix (move 10 units in X, 5 in Y)
    Matrix3d transform = Matrix3d.Displacement(new Vector3d(10, 5, 0));
    
    // Apply transformation
    ent.TransformBy(transform);
    
    tr.Commit();
}
```

### Example 5: Finding Intersections
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent1 = tr.GetObject(entityId1, OpenMode.ForRead) as Entity;
    Entity ent2 = tr.GetObject(entityId2, OpenMode.ForRead) as Entity;
    
    Point3dCollection intersectionPoints = new Point3dCollection();
    
    // Find intersections (Extend mode: None, Both, First, Second)
    ent1.IntersectWith(ent2, Intersect.OnBothOperands, intersectionPoints, IntPtr.Zero, IntPtr.Zero);
    
    foreach (Point3d pt in intersectionPoints)
    {
        ed.WriteMessage($"\nIntersection at: {pt}");
    }
    
    tr.Commit();
}
```

### Example 6: Iterating Through All Entities
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
        OpenMode.ForRead) as BlockTableRecord;
    
    foreach (ObjectId objId in modelSpace)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        ed.WriteMessage($"\nEntity Type: {ent.GetType().Name}");
        ed.WriteMessage($"\n  Layer: {ent.Layer}");
        ed.WriteMessage($"\n  Color Index: {ent.ColorIndex}");
    }
    
    tr.Commit();
}
```

### Example 7: Filtering Entities by Type
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
    
    // Count entities by type
    int lineCount = 0;
    int circleCount = 0;
    int textCount = 0;
    
    foreach (ObjectId objId in btr)
    {
        Entity ent = tr.GetObject(objId, OpenMode.ForRead) as Entity;
        
        if (ent is Line) lineCount++;
        else if (ent is Circle) circleCount++;
        else if (ent is DBText || ent is MText) textCount++;
    }
    
    ed.WriteMessage($"\nLines: {lineCount}, Circles: {circleCount}, Text: {textCount}");
    
    tr.Commit();
}
```

## Common Patterns

### Color Assignment
```csharp
// By color index (ACI - AutoCAD Color Index)
ent.ColorIndex = 1; // Red
ent.ColorIndex = 256; // ByLayer
ent.ColorIndex = 0; // ByBlock

// By true color
ent.Color = Color.FromRgb(255, 0, 0); // Red

// By color book
ent.Color = Color.FromColorIndex(ColorMethod.ByAci, 1);
```

### Layer Assignment
```csharp
// By layer name
ent.Layer = "0"; // Layer 0

// By layer ObjectId
LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
if (lt.Has("MyLayer"))
{
    ent.LayerId = lt["MyLayer"];
}
```

## Related Objects
- [DBObject](DBObject.md) - Base class for Entity
- [Curve](Curve.md) - Base class for curve entities
- [Line](Line.md) - Derived entity type
- [Circle](Circle.md) - Derived entity type
- [BlockReference](BlockReference.md) - Derived entity type
- [BlockTableRecord](BlockTableRecord.md) - Container for entities

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Entity)
- [Entity Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/)
