# UcsTable Class

## Overview
The `UcsTable` class is a symbol table that contains all User Coordinate System (UCS) definitions in an AutoCAD drawing.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ UcsTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a UCS exists by name |
| `this[string]` | `ObjectId` | Gets UCS ObjectId by name (indexer) |

## Code Examples

### Example 1: Listing All UCS Definitions
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    UcsTable ut = tr.GetObject(db.UcsTableId, OpenMode.ForRead) as UcsTable;
    
    ed.WriteMessage("\nUCS definitions in drawing:");
    
    foreach (ObjectId ucsId in ut)
    {
        UcsTableRecord utr = tr.GetObject(ucsId, OpenMode.ForRead) as UcsTableRecord;
        
        ed.WriteMessage($"\n  {utr.Name}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains UcsTableId
- UcsTableRecord - UCS definition

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_UcsTable)
