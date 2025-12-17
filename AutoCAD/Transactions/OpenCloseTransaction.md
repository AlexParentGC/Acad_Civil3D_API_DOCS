# OpenCloseTransaction

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

`OpenCloseTransaction` is a simplified transaction pattern that automatically closes objects when the transaction ends. It's designed for scenarios where you want automatic resource management without explicitly managing object states.

**Key Difference from Transaction:** Objects opened with `OpenCloseTransaction` are automatically closed when the transaction commits or aborts, reducing boilerplate code.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DisposableWrapper
          └─ Transaction
              └─ OpenCloseTransaction
```

## Key Methods

Inherits all methods from `Transaction`, plus:

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetObject(ObjectId, OpenMode)` | `DBObject` | Opens object; auto-closes on commit/abort |
| `GetObject(ObjectId, OpenMode, bool)` | `DBObject` | Opens object with erased option |

## When to Use

| Use OpenCloseTransaction | Use Standard Transaction |
|--------------------------|--------------------------|
| Simple read operations | Complex object manipulation |
| Quick property queries | Creating new objects |
| Batch reading | Nested transactions |
| Minimal object interaction | Long-lived object references |

## Common Usage Patterns

### 1. Basic OpenCloseTransaction

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("OCTBASIC")]
public void OpenCloseTransactionBasic()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (OpenCloseTransaction tr = db.TransactionManager.StartOpenCloseTransaction())
    {
        // Open layer table - will auto-close when transaction ends
        LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
        
        ed.WriteMessage("\n=== Layers ===");
        foreach (ObjectId layerId in lt)
        {
            LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
            ed.WriteMessage($"\n{ltr.Name}");
            // ltr automatically closed when transaction ends
        }
        
        tr.Commit();
    }
}
```

### 2. Reading Multiple Object Types

```csharp
[CommandMethod("OCTREAD")]
public void ReadMultipleObjectTypes()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (OpenCloseTransaction tr = db.TransactionManager.StartOpenCloseTransaction())
    {
        // All objects auto-close at end
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
        LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
        
        ed.WriteMessage($"\nBlocks: {bt.Count}");
        ed.WriteMessage($"\nLayers: {lt.Count}");
        ed.WriteMessage($"\nLinetypes: {ltt.Count}");
        
        tr.Commit();
    }
}
```

### 3. Querying Entity Properties

```csharp
[CommandMethod("OCTQUERY")]
public void QueryEntityProperties()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select an entity
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect an entity: ");
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (OpenCloseTransaction tr = db.TransactionManager.StartOpenCloseTransaction())
    {
        // Object automatically closed after reading
        Entity ent = tr.GetObject(per.ObjectId, OpenMode.ForRead) as Entity;
        
        ed.WriteMessage($"\nEntity Type: {ent.GetType().Name}");
        ed.WriteMessage($"\nLayer: {ent.Layer}");
        ed.WriteMessage($"\nColor: {ent.Color}");
        ed.WriteMessage($"\nLinetype: {ent.Linetype}");
        
        tr.Commit();
    }
}
```

### 4. Batch Property Reading

```csharp
[CommandMethod("OCTBATCH")]
public void BatchPropertyReading()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Select multiple entities
    PromptSelectionResult psr = ed.GetSelection();
    if (psr.Status != PromptStatus.OK) return;
    
    using (OpenCloseTransaction tr = db.TransactionManager.StartOpenCloseTransaction())
    {
        int circleCount = 0;
        int lineCount = 0;
        int otherCount = 0;
        
        foreach (SelectedObject so in psr.Value)
        {
            // Each object auto-closes after iteration
            Entity ent = tr.GetObject(so.ObjectId, OpenMode.ForRead) as Entity;
            
            if (ent is Circle) circleCount++;
            else if (ent is Line) lineCount++;
            else otherCount++;
        }
        
        ed.WriteMessage($"\nCircles: {circleCount}");
        ed.WriteMessage($"\nLines: {lineCount}");
        ed.WriteMessage($"\nOther: {otherCount}");
        
        tr.Commit();
    }
}
```

### 5. Comparison: Standard vs OpenClose Transaction

```csharp
[CommandMethod("COMPARETR")]
public void CompareTransactions()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    ed.WriteMessage("\n=== Standard Transaction ===");
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
        
        foreach (ObjectId layerId in lt)
        {
            LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
            ed.WriteMessage($"\n{ltr.Name}");
            // Must explicitly manage object lifetime if needed
        }
        
        tr.Commit();
    }
    
    ed.WriteMessage("\n\n=== OpenClose Transaction ===");
    using (OpenCloseTransaction tr = db.TransactionManager.StartOpenCloseTransaction())
    {
        LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
        
        foreach (ObjectId layerId in lt)
        {
            LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
            ed.WriteMessage($"\n{ltr.Name}");
            // Objects automatically closed - cleaner code
        }
        
        tr.Commit();
    }
}
```

### 6. Reading Database Statistics

```csharp
[CommandMethod("DBSTATS")]
public void DatabaseStatistics()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (OpenCloseTransaction tr = db.TransactionManager.StartOpenCloseTransaction())
    {
        // Read multiple symbol tables efficiently
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
        LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
        TextStyleTable tst = tr.GetObject(db.TextStyleTableId, OpenMode.ForRead) as TextStyleTable;
        DimStyleTable dst = tr.GetObject(db.DimStyleTableId, OpenMode.ForRead) as DimStyleTable;
        
        ed.WriteMessage("\n=== Database Statistics ===");
        ed.WriteMessage($"\nBlock Definitions: {bt.Count}");
        ed.WriteMessage($"\nLayers: {lt.Count}");
        ed.WriteMessage($"\nLinetypes: {ltt.Count}");
        ed.WriteMessage($"\nText Styles: {tst.Count}");
        ed.WriteMessage($"\nDimension Styles: {dst.Count}");
        
        // Count entities in model space
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForRead) as BlockTableRecord;
        
        int entityCount = 0;
        foreach (ObjectId id in btr)
        {
            entityCount++;
        }
        
        ed.WriteMessage($"\nEntities in Model Space: {entityCount}");
        
        tr.Commit();
    }
}
```

## Advantages

✅ **Automatic cleanup**: Objects close automatically  
✅ **Less code**: No manual object management  
✅ **Safer**: Reduces resource leaks  
✅ **Cleaner**: Better for read-heavy operations

## Limitations

❌ **Not for object creation**: Use standard `Transaction` for `AddNewlyCreatedDBObject`  
❌ **Limited control**: Can't keep objects open after transaction  
❌ **Read-focused**: Best for queries, not complex modifications

## Best Practices

1. **Use for read operations**: Ideal for querying properties and statistics
2. **Batch reads**: Efficiently read multiple objects without manual cleanup
3. **Still commit**: Always call `Commit()` even for read-only operations
4. **Don't mix patterns**: Choose one transaction type per operation
5. **Short-lived**: Keep transactions brief for better performance

## Performance Comparison

| Operation | Standard Transaction | OpenCloseTransaction |
|-----------|---------------------|---------------------|
| Read 100 layers | ⚡ Fast | ⚡ Fast |
| Create 100 circles | ⚡ Fast | ❌ Not supported |
| Query properties | ⚡ Fast | ⚡⚡ Slightly faster |
| Memory usage | 🔵 Normal | 🟢 Lower (auto-cleanup) |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `eNotOpenForWrite` | Trying to modify in read mode | Open with `OpenMode.ForWrite` |
| `eNoActiveTransactions` | Accessing after transaction ends | Keep operations within `using` block |

## Decision Guide

**Use OpenCloseTransaction when:**
- Reading object properties
- Querying database statistics
- Batch reading operations
- Simple, short-lived transactions

**Use Standard Transaction when:**
- Creating new objects
- Complex object modifications
- Need to keep objects open
- Nested transaction scenarios

## Related Classes

- [Transaction](Transaction.md) - Standard transaction with manual control
- [TransactionManager](TransactionManager.md) - Creates and manages transactions
- [DBObject](../BaseClasses/DBObject.md) - Base class for database objects
- [Database](../Core/Database.md) - Contains transaction manager

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-8F3D8F3E-8E0A-4C3C-8787-862EAEF1DB3F)
