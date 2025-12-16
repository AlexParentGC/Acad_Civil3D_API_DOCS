# LinetypeTable Class

## Overview
The `LinetypeTable` class is a symbol table that contains all linetype definitions in an AutoCAD drawing.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ LinetypeTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a linetype exists by name |
| `this[string]` | `ObjectId` | Gets linetype ObjectId by name (indexer) |

## Code Examples

### Example 1: Listing All Linetypes
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
    
    ed.WriteMessage("\nLinetypes in drawing:");
    
    foreach (ObjectId linetypeId in ltt)
    {
        LinetypeTableRecord lttr = tr.GetObject(linetypeId, OpenMode.ForRead) as LinetypeTableRecord;
        
        ed.WriteMessage($"\n  {lttr.Name}");
    }
    
    tr.Commit();
}
```

### Example 2: Checking if Linetype Exists
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    LinetypeTable ltt = tr.GetObject(db.LinetypeTableId, OpenMode.ForRead) as LinetypeTable;
    
    if (ltt.Has("DASHED"))
    {
        ed.WriteMessage("\nDASHED linetype is available");
    }
    else
    {
        ed.WriteMessage("\nDASHED linetype not found - may need to load acad.lin");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains LinetypeTableId
- [Entity](../BaseClasses/Entity.md) - Entities have Linetype property
- LinetypeTableRecord - Linetype definition

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_LinetypeTable)
