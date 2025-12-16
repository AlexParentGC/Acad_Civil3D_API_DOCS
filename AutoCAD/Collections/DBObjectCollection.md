# DBObjectCollection Class

## Overview
`DBObjectCollection` is a container that holds references to actual `DBObject` instances (not just their IDs). It is typically used for operations that involve non-database-resident objects (objects created in memory but not yet added to the database), such as `Entity.Explode()` or `Curve.GetSplitCurves()`.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Count` | `int` | Number of objects in the collection. |
| `this[int]` | `DBObject` | Indexer to get object at index. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Add(DBObject)` | `int` | Adds an object to the collection. |
| `Clear()` | `void` | Clears the collection (doesn't dispose objects). |
| `Contains(DBObject)` | `bool` | Checks if object is in list. |
| `CopyTo(DBObject[], int)` | `void` | Copies to array. |
| `Dispose()` | `void` | Disposes the collection container. |

## Code Examples

### Example 1: Exploding an Entity
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    Entity ent = tr.GetObject(entId, OpenMode.ForRead) as Entity;
    
    // Create specific collection for results
    DBObjectCollection explodedParts = new DBObjectCollection();
    
    // Explode into the collection
    ent.Explode(explodedParts);
    
    ed.WriteMessage($"\nExploded into {explodedParts.Count} pieces.");
    
    // Add new parts to database
    BlockTableRecord btr = tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
    foreach (Entity part in explodedParts)
    {
        btr.AppendEntity(part);
        tr.AddNewlyCreatedDBObject(part, true);
    }
    
    tr.Commit();
}
```

### Example 2: Splitting Curves
```csharp
Curve curve = ...;
Point3dCollection splitPoints = ...;

DBObjectCollection newCurves = new DBObjectCollection();

// Split curve at points
// Resulting curves are added to newCurves
curve.GetSplitCurves(splitPoints, newCurves);
```

### Example 3: Creating Regions
```csharp
// Region.CreateFromCurves requires a DBObjectCollection input
DBObjectCollection curves = new DBObjectCollection();
curves.Add(new Line(new Point3d(0,0,0), new Point3d(10,0,0)));
curves.Add(new Line(new Point3d(10,0,0), new Point3d(10,10,0)));
// (add more closing curves)

DBObjectCollection regions = Region.CreateFromCurves(curves);
```

### Example 4: Proper Disposal
```csharp
// IMPORTANT: DBObjectCollection is IDisposable
using (DBObjectCollection list = new DBObjectCollection())
{
    // fill list
    // use list
} // Collection is disposed here
```

### Example 5: Handling Memory Objects
```csharp
// Objects in DBObjectCollection are often NOT in the database yet.
// If you don't add them to the database, you must Dispose them manually!

DBObjectCollection parts = new DBObjectCollection();
someEntity.Explode(parts);

// ... decide not to use them ...

foreach (DBObject obj in parts)
{
    obj.Dispose(); // Memory cleanup
}
parts.Dispose(); // Container cleanup
```

### Example 6: Filtering by Type
```csharp
foreach (DBObject obj in collection)
{
    if (obj is Line line)
    {
        // Process line
    }
    else if (obj is Arc arc)
    {
        // Process arc
    }
}
```

### Example 7: Converting to List
```csharp
// LINQ helper
List<Entity> entityList = collection.Cast<Entity>().ToList();
```

### Example 8: Removing Items
```csharp
if (collection.Contains(specificObj))
{
    collection.Remove(specificObj);
}
```

## Best Practices
1. **Memory Management**: This is the #1 pitfall. `DBObjectCollection` does **NOT** own the objects inside it. If those objects are not physically added to the Database (via `AppendEntity`), you are responsible for calling `.Dispose()` on every object inside, OR the memory leaks.
2. **Dispose Collection**: The collection itself uses unmanaged resources, so always use `using` or `Dispose()`.
3. **Use API Types**: Don't use `List<DBObject>` when an API method expects `DBObjectCollection` - you must use the AutoCAD type.

## Related Objects
- [DBObject](../BaseClasses/DBObject.md) - The item type.
- [ObjectIdCollection](ObjectIdCollection.md) - Collection of references (handles).

## References
- [Autodesk DBObjectCollection Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DBObjectCollection)
