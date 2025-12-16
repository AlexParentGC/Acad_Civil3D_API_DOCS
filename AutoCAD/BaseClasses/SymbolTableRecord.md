# SymbolTableRecord Class

## Overview
`SymbolTableRecord` is the abstract base class for all objects stored in a `SymbolTable`. Common examples include `LayerTableRecord` (Layers), `BlockTableRecord` (Block Definitions), `LinetypeTableRecord`, and `TextStyleTableRecord`. It defines the common properties for named table items, such as the entry name.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTableRecord
              ├─ LayerTableRecord
              ├─ BlockTableRecord
              ├─ LinetypeTableRecord
              ├─ TextStyleTableRecord
              ├─ DimStyleTableRecord
              ├─ RegAppTableRecord
              ├─ UcsTableRecord
              ├─ ViewTableRecord
              └─ ViewportTableRecord
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | The name of the record (e.g., Layer name "0"). |
| `IsDependent` | `bool` | True if the record belongs to an XRef. |
| `IsResolved` | `bool` | True if the XRef record is resolved. |
| `IsRenamable` | `bool` | True if the record can be renamed. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetName()` | `string` | Gets the name safely. |

## Code Examples

### Example 1: Creating a New Record (Generic)
```csharp
// Typically you instantiate a derived class, not SymbolTableRecord directly
LayerTableRecord layer = new LayerTableRecord();
layer.Name = "MyNewLayer";
// Set other specific properties...
```

### Example 2: Renaming a Record
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTableRecord rec = tr.GetObject(recordId, OpenMode.ForWrite) as SymbolTableRecord;
    
    if (rec.IsRenamable) // Check system protection (e.g., Layer "0")
    {
        rec.Name = "RenamedRecord";
    }
    
    tr.Commit();
}
```

### Example 3: Checking XRef Status
```csharp
public void CheckDependency(SymbolTableRecord rec)
{
    if (rec.IsDependent)
    {
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\n{rec.Name} is an XRef dependent record.");
    }
}
```

### Example 4: Iterating a BlockTableRecord (Inherited)
```csharp
// BlockTableRecord is a SymbolTableRecord, but also a container!
// Note: Only BlockTableRecord behaves as an Entity container.
// Other records (Layer, Linetype) are just definitions.
```

### Example 5: Handling Name Validation
```csharp
public void SetName(SymbolTableRecord rec, string newName)
{
    try
    {
        // SymbolTableRecord validates names (no invalid chars)
        rec.Name = newName;
    }
    catch (Exception ex)
    {
        // Handle invalid name error
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage("\nInvalid name characters.");
    }
}
```

### Example 6: Cloning a Record
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTableRecord original = tr.GetObject(id, OpenMode.ForRead) as SymbolTableRecord;
    SymbolTableRecord clone = original.Clone() as SymbolTableRecord;
    
    clone.Name = original.Name + "_Copy";
    
    // You would then add 'clone' to the owner table...
    tr.Commit();
}
```

### Example 7: Using as DBObject
```csharp
// Since it inherits DBObject, all DBObject methods apply
SymbolTableRecord rec = tr.GetObject(id, OpenMode.ForRead) as SymbolTableRecord;
ObjectId ownerId = rec.OwnerId; // The SymbolTable (e.g., LayerTable)
```

### Example 8: Identifying Record Type
```csharp
if (rec is LayerTableRecord) return "Layer";
if (rec is BlockTableRecord) return "Block";
if (rec is LinetypeTableRecord) return "Linetype";
```

## Best Practices
1. **Name Validation**: Be aware that setting `Name` throws exceptions if the name contains invalid characters (`<`, `>`, `/`, `:`, etc.).
2. **XRef Safety**: dependent records (`IsDependent == true`) are generally read-only; attempting to modify them or rename them will fail.
3. **Renamable Check**: Always check `IsRenamable` before changing `Name` (some system records like "0" or "Defpoints" are locked).

## Related Objects
- [SymbolTable](SymbolTable.md) - The container for these records.
- [LayerTable](../SymbolTables/LayerTable.md) - Example table.
- [LayerTableRecord](../SymbolTables/LayerTableRecord.md) - Example derived record.

## References
- [Autodesk SymbolTableRecord Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_SymbolTableRecord)
