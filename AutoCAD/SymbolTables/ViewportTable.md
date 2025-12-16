# ViewportTable Class

## Overview
The `ViewportTable` class is a symbol table that contains viewport configurations in an AutoCAD drawing.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ ViewportTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a viewport config exists by name |
| `this[string]` | `ObjectId` | Gets viewport ObjectId by name (indexer) |

## Code Examples

### Example 1: Accessing Viewport Table
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    ViewportTable vpt = tr.GetObject(db.ViewportTableId, OpenMode.ForRead) as ViewportTable;
    
    ed.WriteMessage("\nViewport configurations:");
    
    foreach (ObjectId vpId in vpt)
    {
        ViewportTableRecord vptr = tr.GetObject(vpId, OpenMode.ForRead) as ViewportTableRecord;
        
        ed.WriteMessage($"\n  {vptr.Name}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains ViewportTableId
- ViewportTableRecord - Viewport configuration

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_ViewportTable)
