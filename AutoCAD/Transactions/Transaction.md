# Transaction

**Namespace:** `Autodesk.AutoCAD.DatabaseServices`  
**Assembly:** `AcDbMgd.dll`

## Overview

The `Transaction` class is the fundamental mechanism for safely accessing and modifying AutoCAD database objects. Transactions ensure data integrity by providing atomic operations - either all changes succeed or all are rolled back.

**Key Concept:** In AutoCAD .NET API, you must open database objects within a transaction to read or modify them. This prevents corruption and ensures thread safety.

## Class Hierarchy

```
Object
  └─ RXObject
      └─ DisposableWrapper
          └─ Transaction
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `AutoDelete` | `bool` | Gets/sets whether transaction auto-deletes after commit/abort |
| `TransactionManager` | `TransactionManager` | Gets the transaction manager that created this transaction |

## Key Methods

### Transaction Lifecycle

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Commit()` | `void` | Commits all changes made during the transaction |
| `Abort()` | `void` | Rolls back all changes made during the transaction |
| `Dispose()` | `void` | Disposes the transaction (calls Abort if not committed) |

### Object Access

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetObject(ObjectId, OpenMode)` | `DBObject` | Opens a database object for reading or writing |
| `GetObject(ObjectId, OpenMode, bool)` | `DBObject` | Opens object with option to open erased objects |
| `AddNewlyCreatedDBObject(DBObject, bool)` | `void` | Adds a newly created object to the transaction |

## Common Usage Patterns

### 1. Basic Transaction Pattern

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;

[CommandMethod("BASICTRANS")]
public void BasicTransactionExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        try
        {
            // Open model space for writing
            BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
            BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
                OpenMode.ForWrite) as BlockTableRecord;
            
            // Create a circle
            Circle circle = new Circle(new Point3d(0, 0, 0), Vector3d.ZAxis, 10.0);
            
            // Add to database
            btr.AppendEntity(circle);
            tr.AddNewlyCreatedDBObject(circle, true);
            
            // Commit changes
            tr.Commit();
        }
        catch (System.Exception ex)
        {
            doc.Editor.WriteMessage($"\nError: {ex.Message}");
            // Transaction will auto-abort in Dispose if not committed
        }
    }
}
```

### 2. Reading Object Properties

```csharp
[CommandMethod("READLAYERS")]
public void ReadLayersExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Open layer table for reading
        LayerTable lt = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as LayerTable;
        
        ed.WriteMessage("\n=== Layers in Drawing ===");
        foreach (ObjectId layerId in lt)
        {
            LayerTableRecord ltr = tr.GetObject(layerId, OpenMode.ForRead) as LayerTableRecord;
            ed.WriteMessage($"\nLayer: {ltr.Name}, Color: {ltr.Color.ColorIndex}");
        }
        
        tr.Commit(); // Good practice even for read-only operations
    }
}
```

### 3. Modifying Existing Objects

```csharp
[CommandMethod("MODIFYENTITY")]
public void ModifyEntityExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    // Prompt user to select an entity
    PromptEntityOptions peo = new PromptEntityOptions("\nSelect a circle to modify: ");
    peo.SetRejectMessage("\nMust be a circle.");
    peo.AddAllowedClass(typeof(Circle), true);
    
    PromptEntityResult per = ed.GetEntity(peo);
    if (per.Status != PromptStatus.OK) return;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        // Open circle for writing
        Circle circle = tr.GetObject(per.ObjectId, OpenMode.ForWrite) as Circle;
        
        // Modify properties
        circle.Radius = circle.Radius * 2.0;
        circle.ColorIndex = 1; // Red
        
        ed.WriteMessage($"\nCircle radius doubled to {circle.Radius}");
        
        tr.Commit();
    }
}
```

### 4. Creating Multiple Objects

```csharp
[CommandMethod("CREATEGRID")]
public void CreateGridExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForWrite) as BlockTableRecord;
        
        // Create 5x5 grid of circles
        for (int x = 0; x < 5; x++)
        {
            for (int y = 0; y < 5; y++)
            {
                Circle circle = new Circle(
                    new Point3d(x * 20, y * 20, 0), 
                    Vector3d.ZAxis, 
                    5.0
                );
                
                btr.AppendEntity(circle);
                tr.AddNewlyCreatedDBObject(circle, true);
            }
        }
        
        tr.Commit();
        doc.Editor.WriteMessage("\nCreated 25 circles in grid pattern");
    }
}
```

### 5. Nested Transactions (Advanced)

```csharp
[CommandMethod("NESTEDTRANS")]
public void NestedTransactionExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    
    using (Transaction tr1 = db.TransactionManager.StartTransaction())
    {
        BlockTable bt = tr1.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
        BlockTableRecord btr = tr1.GetObject(bt[BlockTableRecord.ModelSpace], 
            OpenMode.ForWrite) as BlockTableRecord;
        
        // Create first circle
        Circle circle1 = new Circle(new Point3d(0, 0, 0), Vector3d.ZAxis, 10.0);
        btr.AppendEntity(circle1);
        tr1.AddNewlyCreatedDBObject(circle1, true);
        
        // Nested transaction
        using (Transaction tr2 = db.TransactionManager.StartTransaction())
        {
            // Create second circle
            Circle circle2 = new Circle(new Point3d(30, 0, 0), Vector3d.ZAxis, 10.0);
            btr.AppendEntity(circle2);
            tr2.AddNewlyCreatedDBObject(circle2, true);
            
            tr2.Commit(); // Nested commit
        }
        
        tr1.Commit(); // Outer commit
    }
}
```

### 6. Error Handling and Rollback

```csharp
[CommandMethod("SAFETRANS")]
public void SafeTransactionExample()
{
    Document doc = Application.DocumentManager.MdiActiveDocument;
    Database db = doc.Database;
    Editor ed = doc.Editor;
    
    using (Transaction tr = db.TransactionManager.StartTransaction())
    {
        try
        {
            BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
            BlockTableRecord btr = tr.GetObject(bt[BlockTableRecord.ModelSpace], 
                OpenMode.ForWrite) as BlockTableRecord;
            
            // Simulate potential error
            Circle circle = new Circle(new Point3d(0, 0, 0), Vector3d.ZAxis, -5.0); // Invalid radius!
            
            btr.AppendEntity(circle);
            tr.AddNewlyCreatedDBObject(circle, true);
            
            tr.Commit();
        }
        catch (Autodesk.AutoCAD.Runtime.Exception acEx)
        {
            ed.WriteMessage($"\nAutoCAD Error: {acEx.Message}");
            // Transaction automatically aborts in Dispose
        }
        catch (System.Exception ex)
        {
            ed.WriteMessage($"\nGeneral Error: {ex.Message}");
            // Transaction automatically aborts in Dispose
        }
    }
    
    ed.WriteMessage("\nTransaction completed (changes rolled back if error occurred)");
}
```

## Best Practices

1. **Always use `using` statement**: Ensures transaction is disposed even if exceptions occur
2. **Commit explicitly**: Call `Commit()` when all operations succeed
3. **Let Dispose handle abort**: Don't call `Abort()` explicitly; `Dispose()` will abort if not committed
4. **Open objects with minimum permissions**: Use `OpenMode.ForRead` when possible
5. **Minimize transaction scope**: Keep transactions short to reduce locking
6. **Handle exceptions**: Wrap transaction code in try-catch blocks
7. **Don't cross document boundaries**: Each document has its own transaction manager

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `eNoActiveTransactions` | Accessing object without transaction | Start a transaction first |
| `eOnLockedLayer` | Modifying object on locked layer | Unlock layer or open for read only |
| `eWasErased` | Accessing erased object | Use `GetObject` with `openErased: true` |
| `eNotOpenForWrite` | Modifying read-only object | Open with `OpenMode.ForWrite` |

## Related Classes

- [TransactionManager](TransactionManager.md) - Manages transaction lifecycle
- [OpenCloseTransaction](OpenCloseTransaction.md) - Simplified transaction pattern
- [DBObject](../BaseClasses/DBObject.md) - Base class for all database objects
- [Database](../Core/Database.md) - Drawing database

## See Also

- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-C0869B95-08F0-4C3C-8787-862EAEF1DB3F)
