# TextStyleTable Class

## Overview
The `TextStyleTable` class is a symbol table that contains all text style definitions in an AutoCAD drawing.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ TextStyleTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a text style exists by name |
| `this[string]` | `ObjectId` | Gets text style ObjectId by name (indexer) |

## Code Examples

### Example 1: Listing All Text Styles
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    TextStyleTable tst = tr.GetObject(db.TextStyleTableId, OpenMode.ForRead) as TextStyleTable;
    
    ed.WriteMessage("\nText styles in drawing:");
    
    foreach (ObjectId styleId in tst)
    {
        TextStyleTableRecord tstr = tr.GetObject(styleId, OpenMode.ForRead) as TextStyleTableRecord;
        
        ed.WriteMessage($"\n  {tstr.Name}");
        ed.WriteMessage($" - Font: {tstr.FileName}");
    }
    
    tr.Commit();
}
```

### Example 2: Creating a New Text Style
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    TextStyleTable tst = tr.GetObject(db.TextStyleTableId, OpenMode.ForWrite) as TextStyleTable;
    
    if (!tst.Has("MyStyle"))
    {
        TextStyleTableRecord tstr = new TextStyleTableRecord();
        tstr.Name = "MyStyle";
        tstr.FileName = "arial.ttf";
        tstr.TextSize = 0.0; // Variable height
        
        tst.Add(tstr);
        tr.AddNewlyCreatedDBObject(tstr, true);
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains TextStyleTableId
- [DBText](../Entities/Text/DBText.md) - Uses text styles
- [MText](../Entities/Text/MText.md) - Uses text styles
- TextStyleTableRecord - Text style definition

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_TextStyleTable)
