# ObjectIdCollection Class

## Overview
`ObjectIdCollection` is a specialized container designed to hold a list of `ObjectId` structures. It is extensively used in the AutoCAD API for operations that require multiple object references, such as bulk erasing code, transaction management, and passing selection sets to API methods.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Count` | `int` | Gets the number of IDs in the collection. |
| `this[int]` | `ObjectId` | Indexer to get/set ID at specific index. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Add(ObjectId)` | `int` | Adds an ID to the end. Returns constant 0. |
| `Remove(ObjectId)` | `void` | Removes the first occurrence of the ID. |
| `Contains(ObjectId)` | `bool` | Checks if ID exists in collection. |
| `Clear()` | `void` | Removing all elements. |
| `ToArray()` | `ObjectId[]` | Copies elements to a new array. |
| `CopyTo(ObjectId[], int)` | `void` | Copies elements to an existing array. |

## Code Examples

### Example 1: Creating and Populating
```csharp
ObjectIdCollection ids = new ObjectIdCollection();

// Assuming you have some object IDs
ids.Add(lineId);
ids.Add(circleId);
ids.Add(arcId);

Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nCollection has {ids.Count} items.");
```

### Example 2: Using with Transaction (Bulk Erase)
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ObjectIdCollection idsToErase = new ObjectIdCollection();
    
    // ... gather IDs ...
    idsToErase.Add(someId);
    
    foreach (ObjectId id in idsToErase)
    {
        DBObject obj = tr.GetObject(id, OpenMode.ForWrite);
        obj.Erase();
    }
    
    tr.Commit();
}
```

### Example 3: Converting SelectionSet to Collection
```csharp
PromptSelectionResult selRes = ed.GetSelection();
if (selRes.Status == PromptStatus.OK)
{
    // SelectionSet.GetObjectIds returns ObjectId[]
    ObjectId[] idArray = selRes.Value.GetObjectIds();
    
    // Create collection from array
    ObjectIdCollection col = new ObjectIdCollection(idArray);
    
    ed.WriteMessage($"\nConverted {col.Count} selected items.");
}
```

### Example 4: Iterating
```csharp
foreach (ObjectId id in myCollection)
{
    // Perform check
    if (id.IsNull) continue;
    
    // Process ID
}
```

### Example 5: Checking Membership
```csharp
if (ids.Contains(targetId))
{
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nID is already in the list.");
}
else
{
    ids.Add(targetId);
}
```

### Example 6: Removing Items
```csharp
// Remove specific ID
ids.Remove(idToRemove);

// Remove via loop (backwards is safest for list modification, though Remove handles it)
// ObjectIdCollection handles removal robustly, but classic logic applies.
ids.RemoveAt(0); // If implemented, otherwise use Remove(obj) behavior
```

### Example 7: Using with Purge
```csharp
// Common API pattern: passing collection to Purge
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ObjectIdCollection idsToPurge = new ObjectIdCollection();
    idsToPurge.Add(layerId);
    
    // db.Purge(idsToPurge); // Example of API method consuming it
    
    tr.Commit();
}
```

### Example 8: Copying to Array
```csharp
ObjectId[] simpleArray = new ObjectId[ids.Count];
ids.CopyTo(simpleArray, 0);

// Or simply
ObjectId[] easyArray = ids.ToArray();
```

## Best Practices
1. **Dispose Not Required**: Unlike `DBObject`, `ObjectIdCollection` is a managed wrapper around a simple list and does not strictly require disposal, but usually implements `IDisposable` in COM wrappers. In .NET, it's generally safe, but `using` blocks are good practice if available. (Check `IDisposable` implementation in specific version).
2. **Performance**: For massive lists (100k+), standard `List<ObjectId>` might be slightly faster, but API methods specifically require `ObjectIdCollection`. Use the native collection when interacting with AutoCAD methods.
3. **Null Handling**: It *can* contain null `ObjectId`s, so filter them if necessary.

## Related Objects
- [ObjectId](../BaseClasses/ObjectId.md) - The item type.
- [DBObjectCollection](DBObjectCollection.md) - Collection of objects themselves.

## References
- [Autodesk ObjectIdCollection Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ObjectIdCollection)
