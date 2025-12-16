# ObjectId Struct

## Overview
The `ObjectId` struct is the robust, persistent identifier for any `DBObject` residing in an AutoCAD database. Unlike a memory pointer, an `ObjectId` remains valid across transactions and memory reallocation. It is the primary handle used to retrieve objects from the `TransactionManager`.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `IsNull` | `bool` | True if the ID is not valid/null. |
| `IsValid` | `bool` | True if the ID points to a valid object. |
| `IsErased` | `bool` | True if the target object is erased. |
| `ObjectClass` | `RXClass` | The runtime class type of the target object. |
| `Database` | `Database` | The database owning this object. |
| `Handle` | `Handle` | The persistent handle of the target object. |
| `OriginalDatabase` | `Database` | The database where the object originated (for XRefs). |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetObject(OpenMode)` | `DBObject` | **Deprecated**. Use `Transaction.GetObject` instead. |
| `Equals(object)` | `bool` | Checks equality with another ObjectId. |
| `GetHashCode()` | `int` | Returns hash code for dictionary keys. |
| `ToString()` | `string` | Returns string representation. |

## Code Examples

### Example 1: Checking for Null ObjectId
```csharp
public void CheckId(ObjectId id)
{
    if (id.IsNull)
    {
        // Handle null ID case
        return;
    }
    
    // Proceed with valid ID
}
```

### Example 2: Getting Object Type from ObjectId
```csharp
public void CheckType(ObjectId id)
{
    // Check type without opening the object (FAST)
    if (id.ObjectClass.IsDerivedFrom(RXObject.GetClass(typeof(Curve))))
    {
        // It's a curve (Line, Arc, Polyline, etc.)
    }
}
```

### Example 3: Using ObjectId in Transaction
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Best Practice: Always use Transaction.GetObject
    Entity ent = tr.GetObject(objectId, OpenMode.ForRead) as Entity;
    if (ent != null)
    {
        // Work with entity
    }
    tr.Commit();
}
```

### Example 4: Comparing ObjectIds
```csharp
public bool IsSameObject(ObjectId id1, ObjectId id2)
{
    // Direct comparison operator
    return id1 == id2;
}
```

### Example 5: ObjectId Validity Check
```csharp
public void ValidateId(ObjectId id)
{
    if (!id.IsValid)
    {
        // ID might be from a deleted database or wildly invalid
        throw new ArgumentException("Invalid ObjectId");
    }

    if (id.IsErased)
    {
        // Object exists but is soft-deleted
        // You can still open it with OpenMode.ForRead if needed, but usually skip.
    }
}
```

### Example 6: Extracting Handle
```csharp
public void LogHandle(ObjectId id)
{
    // Handle is the persistent string identifier (e.g., "5A")
    Handle h = id.Handle;
    Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nHandle: {h}");
}
```

### Example 7: Using as Dictionary Key
```csharp
// ObjectId implements GetHashCode and Equals correctly
Dictionary<ObjectId, string> objectTags = new Dictionary<ObjectId, string>();

objectTags[id1] = "Inspected";
if (objectTags.ContainsKey(id1))
{
    // Found
}
```

### Example 8: Checking Origin Database (XRef)
```csharp
public void CheckXRef(ObjectId id)
{
    if (id.Database != id.OriginalDatabase)
    {
        // Object is likely from an XRef
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nXRef Object Detected");
    }
}
```

## Best Practices
1. **Never use `Handle` for runtime logic**: Use `ObjectId` whenever possible as it is faster. Use `Handle` only for persistence between sessions.
2. **Avoid `Open()`**: Do not use `ObjectId.Open()`. Always use `Transaction.GetObject()`.
3. **Check `IsNull`**: Always check `ObjectId.Null` before passing IDs around.
4. **Use `ObjectClass`**: Check object type via `id.ObjectClass` to avoid opening objects just to check their type.

## Related Objects
- [DBObject](DBObject.md) - The object pointed to by ObjectId.
- [Database](../Core/Database.md) - The container of the object.
- [Handle](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_Handle) - Persistent identifier.

## References
- [Autodesk ObjectId Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ObjectId)
