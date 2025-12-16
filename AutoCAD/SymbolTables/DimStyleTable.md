# DimStyleTable Class

## Overview
The `DimStyleTable` class is a symbol table that contains all dimension style definitions in an AutoCAD drawing.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ DimStyleTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a dimension style exists by name |
| `this[string]` | `ObjectId` | Gets dimension style ObjectId by name (indexer) |

## Code Examples

### Example 1: Listing All Dimension Styles
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    DimStyleTable dst = tr.GetObject(db.DimStyleTableId, OpenMode.ForRead) as DimStyleTable;
    
    ed.WriteMessage("\nDimension styles in drawing:");
    
    foreach (ObjectId styleId in dst)
    {
        DimStyleTableRecord dstr = tr.GetObject(styleId, OpenMode.ForRead) as DimStyleTableRecord;
        
        ed.WriteMessage($"\n  {dstr.Name}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains DimStyleTableId
- [Dimension](../Entities/Annotations/Dimension.md) - Uses dimension styles
- DimStyleTableRecord - Dimension style definition

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_DimStyleTable)
