# LayerTable Class

## Overview
The `LayerTable` class is a symbol table that contains all layer definitions in an AutoCAD drawing.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ LayerTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a layer exists by name |
| `this[string]` | `ObjectId` | Gets layer ObjectId by name (indexer) |

## Code Examples

### Example 1: Listing All Layers
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    ed.WriteMessage("\nLayers in drawing:");
    
    foreach (ObjectId layerId in lt)
    {
        LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
        
        ed.WriteMessage($"\n  {ltr.Name}");
        ed.WriteMessage($" - Color: {ltr.Color.ColorIndex}");
        ed.WriteMessage($" - {(ltr.IsFrozen ? "Frozen" : "Thawed")}");
        ed.WriteMessage($" - {(ltr.IsOff ? "Off" : "On")}");
        ed.WriteMessage($" - {(ltr.IsLocked ? "Locked" : "Unlocked")}");
    }
    
    tr.Commit();
}
```

### Example 2: Creating a New Layer
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForWrite) as LayerTable;
    
    if (!lt.Has("MyLayer"))
    {
        LayerTableRecord ltr = new LayerTableRecord();
        ltr.Name = "MyLayer";
        ltr.Color = Color.FromColorIndex(ColorMethod.ByAci, 1); // Red
        
        lt.Add(ltr);
        tr.AddNewlyCreatedDBObject(ltr, true);
        
        ed.WriteMessage("\nLayer 'MyLayer' created");
    }
    
    tr.Commit();
}
```

### Example 3: Modifying Layer Properties
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
    
    if (lt.Has("MyLayer"))
    {
        LayerTableRecord ltr = tr.GetObject(lt["MyLayer"], OpenMode.ForWrite) as LayerTableRecord;
        
        ltr.Color = Color.FromColorIndex(ColorMethod.ByAci, 3); // Green
        ltr.IsFrozen = false;
        ltr.IsOff = false;
        ltr.IsLocked = false;
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains LayerTableId
- [Entity](../BaseClasses/Entity.md) - Entities have Layer property
- LayerTableRecord - Layer definition

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_LayerTable)
