# DBObject Class

## Overview
The `DBObject` class is the fundamental base class for all objects stored in an AutoCAD database. It provides core functionality for object persistence, transactions, handles, and object lifecycle management. Every object in the drawing database—whether graphical (entities) or non-graphical (symbol table records, dictionaries, etc.)—derives from `DBObject`.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          ├─ Entity (all graphical objects)
          ├─ SymbolTableRecord (layers, blocks, etc.)
          ├─ DBDictionary
          ├─ XRecord
          ├─ SymbolTable
          └─ ... (all database objects)
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ObjectId` | `ObjectId` | Gets the ObjectId of this object |
| `Handle` | `Handle` | Gets the unique handle of this object |
| `Database` | `Database` | Gets the database containing this object |
| `IsErased` | `bool` | Gets whether the object has been erased |
| `IsModified` | `bool` | Gets whether the object has been modified |
| `IsNewObject` | `bool` | Gets whether this is a newly created object |
| `IsNotifying` | `bool` | Gets whether the object is notifying reactors |
| `IsReadEnabled` | `bool` | Gets whether the object is open for read |
| `IsWriteEnabled` | `bool` | Gets whether the object is open for write |
| `IsUndoing` | `bool` | Gets whether an undo operation is in progress |
| `ExtensionDictionary` | `ObjectId` | Gets the extension dictionary ObjectId |
| `OwnerId` | `ObjectId` | Gets the ObjectId of the owner object |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Erase()` | `void` | Erases (soft deletes) the object |
| `Erase(bool)` | `void` | Erases or unerases the object |
| `UpgradeOpen()` | `void` | Upgrades object from read to write mode |
| `DowngradeOpen()` | `void` | Downgrades object from write to read mode |
| `Cancel()` | `void` | Cancels changes made to the object |
| `CreateExtensionDictionary()` | `ObjectId` | Creates an extension dictionary |
| `GetRXClass()` | `RXClass` | Gets the runtime class information |
| `Dispose()` | `void` | Releases the object |

## Code Examples

### Example 1: Getting Object Information
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage("\n=== Object Information ===");
    ed.WriteMessage($"\nType: {obj.GetType().Name}");
    ed.WriteMessage($"\nObjectId: {obj.ObjectId}");
    ed.WriteMessage($"\nHandle: {obj.Handle}");
    ed.WriteMessage($"\nDatabase: {obj.Database.Filename}");
    ed.WriteMessage($"\nOwner: {obj.OwnerId}");
    ed.WriteMessage($"\nIs Erased: {obj.IsErased}");
    ed.WriteMessage($"\nIs Modified: {obj.IsModified}");
    ed.WriteMessage($"\nIs New: {obj.IsNewObject}");
    
    tr.Commit();
}
```

### Example 2: Opening Objects for Read and Write
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Open for read
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage($"\nRead enabled: {obj.IsReadEnabled}");
    ed.WriteMessage($"\nWrite enabled: {obj.IsWriteEnabled}");
    
    // Upgrade to write
    obj.UpgradeOpen();
    
    ed.WriteMessage($"\nAfter upgrade - Write enabled: {obj.IsWriteEnabled}");
    
    // Make changes...
    if (obj is Entity ent)
    {
        ent.ColorIndex = 1; // Change to red
    }
    
    // Downgrade back to read
    obj.DowngradeOpen();
    
    ed.WriteMessage($"\nAfter downgrade - Write enabled: {obj.IsWriteEnabled}");
    
    tr.Commit();
}
```

### Example 3: Erasing and Undeleting Objects
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForWrite);
    
    // Soft delete (erase)
    obj.Erase();
    
    ed.WriteMessage($"\nObject erased: {obj.IsErased}");
    
    // Unerase (restore)
    obj.Erase(false);
    
    ed.WriteMessage($"\nObject restored: {!obj.IsErased}");
    
    tr.Commit();
}
```

### Example 4: Working with Extension Dictionaries
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForWrite);
    
    // Check if extension dictionary exists
    if (obj.ExtensionDictionary == ObjectId.Null)
    {
        // Create extension dictionary
        obj.CreateExtensionDictionary();
        ed.WriteMessage("\nCreated extension dictionary");
    }
    
    // Access the extension dictionary
    DBDictionary extDict = tr.GetObject(obj.ExtensionDictionary, OpenMode.ForWrite) as DBDictionary;
    
    // Add custom data
    XRecord xRec = new XRecord();
    ResultBuffer rb = new ResultBuffer(
        new TypedValue((int)DxfCode.Text, "CustomData"),
        new TypedValue((int)DxfCode.Int32, 12345)
    );
    xRec.Data = rb;
    
    extDict.SetAt("MyData", xRec);
    tr.AddNewlyCreatedDBObject(xRec, true);
    
    rb.Dispose();
    
    ed.WriteMessage($"\nExtension dictionary entries: {extDict.Count}");
    
    tr.Commit();
}
```

### Example 5: Checking Object State
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage("\n=== Object State ===");
    ed.WriteMessage($"\nIs Read Enabled: {obj.IsReadEnabled}");
    ed.WriteMessage($"\nIs Write Enabled: {obj.IsWriteEnabled}");
    ed.WriteMessage($"\nIs Erased: {obj.IsErased}");
    ed.WriteMessage($"\nIs Modified: {obj.IsModified}");
    ed.WriteMessage($"\nIs New Object: {obj.IsNewObject}");
    ed.WriteMessage($"\nIs Undoing: {obj.IsUndoing}");
    ed.WriteMessage($"\nIs Notifying: {obj.IsNotifying}");
    
    tr.Commit();
}
```

### Example 6: Canceling Changes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entityId, OpenMode.ForWrite) as Entity;
    
    // Save original color
    short originalColor = ent.ColorIndex;
    ed.WriteMessage($"\nOriginal color: {originalColor}");
    
    // Make changes
    ent.ColorIndex = 1; // Red
    ed.WriteMessage($"\nChanged color to: {ent.ColorIndex}");
    
    // Cancel changes
    ent.Cancel();
    
    ed.WriteMessage($"\nAfter cancel: {ent.ColorIndex}");
    
    // Don't commit - changes are canceled
    tr.Abort();
}
```

### Example 7: Getting Runtime Class Information
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    RXClass rxClass = obj.GetRXClass();
    
    ed.WriteMessage("\n=== Runtime Class Info ===");
    ed.WriteMessage($"\nClass Name: {rxClass.Name}");
    ed.WriteMessage($"\nDXF Name: {rxClass.DxfName}");
    ed.WriteMessage($"\nApp Name: {rxClass.AppName}");
    
    // Check class hierarchy
    RXClass parent = rxClass.MyParent;
    while (parent != null)
    {
        ed.WriteMessage($"\nParent: {parent.Name}");
        parent = parent.MyParent;
    }
    
    tr.Commit();
}
```

### Example 8: Iterating Through All Database Objects
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Iterate through all objects in the database
    Dictionary<string, int> objectCounts = new Dictionary<string, int>();
    
    // Get all entities in model space
    BlockTable bt = tr.GetObject(db.BlockTableId, OpenMode.ForRead) as BlockTable;
    BlockTableRecord modelSpace = tr.GetObject(bt[BlockTableRecord.ModelSpace], OpenMode.ForRead) as BlockTableRecord;
    
    foreach (ObjectId objId in modelSpace)
    {
        DBObject obj = tr.GetObject(objId, OpenMode.ForRead);
        string typeName = obj.GetType().Name;
        
        if (objectCounts.ContainsKey(typeName))
            objectCounts[typeName]++;
        else
            objectCounts[typeName] = 1;
    }
    
    ed.WriteMessage("\n=== Object Type Counts ===");
    foreach (var kvp in objectCounts.OrderByDescending(x => x.Value))
    {
        ed.WriteMessage($"\n{kvp.Key}: {kvp.Value}");
    }
    
    tr.Commit();
}
```

### Example 9: Comparing Objects by Handle
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj1 = tr.GetObject(objectId1, OpenMode.ForRead);
    DBObject obj2 = tr.GetObject(objectId2, OpenMode.ForRead);
    
    // Compare by ObjectId
    bool sameById = obj1.ObjectId == obj2.ObjectId;
    
    // Compare by Handle
    bool sameByHandle = obj1.Handle == obj2.Handle;
    
    ed.WriteMessage($"\nSame by ObjectId: {sameById}");
    ed.WriteMessage($"\nSame by Handle: {sameByHandle}");
    ed.WriteMessage($"\nObj1 Handle: {obj1.Handle}");
    ed.WriteMessage($"\nObj2 Handle: {obj2.Handle}");
    
    tr.Commit();
}
```

### Example 10: Finding Object Owner
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DBObject obj = tr.GetObject(objectId, OpenMode.ForRead);
    
    ed.WriteMessage("\n=== Object Ownership Chain ===");
    ed.WriteMessage($"\nObject: {obj.GetType().Name} ({obj.Handle})");
    
    // Walk up the ownership chain
    ObjectId currentOwnerId = obj.OwnerId;
    int level = 1;
    
    while (!currentOwnerId.IsNull)
    {
        DBObject owner = tr.GetObject(currentOwnerId, OpenMode.ForRead);
        ed.WriteMessage($"\n{new string(' ', level * 2)}Owner {level}: {owner.GetType().Name} ({owner.Handle})");
        
        currentOwnerId = owner.OwnerId;
        level++;
        
        // Prevent infinite loops
        if (level > 10) break;
    }
    
    tr.Commit();
}
```

## Object Lifecycle

### Creation
1. Create new object instance
2. Add to container (BlockTableRecord, SymbolTable, Dictionary, etc.)
3. Call `Transaction.AddNewlyCreatedDBObject()`
4. Commit transaction

### Modification
1. Open object with `OpenMode.ForWrite`
2. Modify properties
3. Commit transaction (or call `Cancel()` to revert)

### Deletion
1. Open object with `OpenMode.ForWrite`
2. Call `Erase()` for soft delete
3. Commit transaction
4. Call `Erase(false)` to unerase if needed

## Open Modes

| Mode | Description | Usage |
|------|-------------|-------|
| `ForRead` | Read-only access | Querying properties, no modifications |
| `ForWrite` | Read and write access | Modifying properties, erasing |
| `ForNotify` | Notification only | Reactor notifications |

## Best Practices

1. **Always Use Transactions**: Never work with DBObjects outside of transactions
2. **Open with Correct Mode**: Use `ForRead` when possible to avoid locking
3. **Upgrade When Needed**: Use `UpgradeOpen()` to change from read to write
4. **Dispose Properly**: Let transactions handle disposal, or use `using` statements
5. **Check IsErased**: Verify object hasn't been deleted before accessing
6. **Handle Exceptions**: Wrap database operations in try-catch blocks
7. **Use Handles for Persistence**: Handles persist across sessions, ObjectIds don't
8. **Extension Dictionaries**: Use for custom data storage on objects
9. **Check Owner**: Understand object ownership before erasing
10. **Reactor Safety**: Be careful with object reactors to avoid circular references

## ObjectId vs Handle

| Feature | ObjectId | Handle |
|---------|----------|--------|
| Persistence | Session only | Permanent |
| Uniqueness | Per session | Per drawing |
| Speed | Fast | Slower (requires lookup) |
| Use Case | Runtime operations | Cross-session references |

## Common Derived Classes

### Graphical Objects
- **Entity** - Base for all graphical objects (lines, circles, text, etc.)
- **Curve** - Base for curve entities

### Non-Graphical Objects
- **SymbolTableRecord** - Layer, block, linetype records, etc.
- **DBDictionary** - Named object dictionaries
- **XRecord** - Custom data storage
- **SymbolTable** - Layer table, block table, etc.

## Related Objects
- [Entity](Entity.md) - Derived class for graphical objects
- [DBDictionary](../Core/DBDictionary.md) - Derived class for dictionaries
- [XRecord](../Core/XRecord.md) - Derived class for custom data
- [Database](../Core/Database.md) - Contains all DBObjects
- ObjectId - Reference to DBObject
- Handle - Permanent identifier for DBObject
- Transaction - Manages DBObject access

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/)
- [DBObject Class Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DBObject)
- [Working with Database Objects](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-C1D6E0F8-8B89-4F1A-9B7E-8F8E9F9F9F9F)
