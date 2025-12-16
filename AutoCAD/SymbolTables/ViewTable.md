# ViewTable Class

## Overview
The `ViewTable` class is a symbol table that contains all named view definitions in an AutoCAD drawing.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ ViewTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a view exists by name |
| `this[string]` | `ObjectId` | Gets view ObjectId by name (indexer) |

## Code Examples

### Example 1: Listing All Named Views
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ViewTable vt = tr.GetObject(db.ViewTableId, OpenMode.ForRead) as ViewTable;
    
    ed.WriteMessage("\nNamed views in drawing:");
    
    foreach (ObjectId viewId in vt)
    {
        ViewTableRecord vtr = tr.GetObject(viewId, OpenMode.ForRead) as ViewTableRecord;
        
        ed.WriteMessage($"\n  {vtr.Name}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains ViewTableId
- ViewTableRecord - Named view definition

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ViewTable)
