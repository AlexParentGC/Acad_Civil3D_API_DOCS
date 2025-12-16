# SymbolTable Class

## Overview
`SymbolTable` is the abstract base class for all symbol tables in the AutoCAD database, such as `BlockTable`, `LayerTable`, `LinetypeTable`, etc. It acts as a specialized dictionary that maps string names (keys) to `ObjectId`s of `SymbolTableRecord`s.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              ├─ BlockTable
              ├─ LayerTable
              ├─ LinetypeTable
              ├─ TextStyleTable
              ├─ DimStyleTable
              ├─ RegAppTable
              ├─ UcsTable
              ├─ ViewTable
              └─ ViewportTable
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `this[string]` | `ObjectId` | Indexer to get record ID by name. |
| `Count` | `int` | Number of records in the table. |

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if a record with the given name exists. |
| `Has(ObjectId)` | `bool` | Checks if a record with the given ID exists in this table. |
| `Add(SymbolTableRecord)` | `ObjectId` | Adds a new record to the table. |
| `GetEnumerator()` | `SymbolTableEnumerator` | Returns enumerator for ObjectIds. |

## Code Examples

### Example 1: Checking for Record Existence
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // Open a derived table (e.g., LayerTable) as SymbolTable
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as SymbolTable;
    
    if (table.Has("MyLayer"))
    {
        // Layer exists
    }
    
    tr.Commit();
}
```

### Example 2: Adding a New Record
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    // 1. Open Table for Write
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForWrite) as SymbolTable;
    
    // 2. Create Record (Derived type)
    LayerTableRecord newRec = new LayerTableRecord();
    newRec.Name = "NewLayer";
    
    // 3. Add to Table
    // Add() returns the new ObjectId
    ObjectId newId = table.Add(newRec);
    
    // 4. Add to Transaction
    tr.AddNewlyCreatedDBObject(newRec, true);
    
    tr.Commit();
}
```

### Example 3: Iterating a SymbolTable
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as SymbolTable;
    
    // Enumerate ObjectIds
    foreach (ObjectId id in table)
    {
        // Open each record
        SymbolTableRecord rec = tr.GetObject(id, OpenMode.ForRead) as SymbolTableRecord;
        Application.DocumentManager.MdiActiveDocument.Editor.WriteMessage($"\nFound: {rec.Name}");
    }
    
    tr.Commit();
}
```

### Example 4: Using Indexer
```csharp
ObjectId id = table["MyLayer"];
// Note: Throws KeyNotFoundException if "MyLayer" doesn't exist.
// Use Has() first if unsure.
```

### Example 5: Case Sensitivity
```csharp
// Symbol table keys are generally case-insensitive in AutoCAD
if (table.Has("mylayer") && table.Has("MYLAYER"))
{
    // These refer to the same record
}
```

### Example 6: Removing Records
```csharp
// SymbolTable does NOT have a Remove() method directly.
// You must open the Record itself and call Erase().
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    SymbolTable table = tr.GetObject(db.LayerTableId, OpenMode.ForRead) as SymbolTable;
    
    if (table.Has("LayerToDelete"))
    {
        ObjectId id = table["LayerToDelete"];
        DBObject rec = tr.GetObject(id, OpenMode.ForWrite);
        rec.Erase(); // This removes it from the table
    }
    
    tr.Commit();
}
```

### Example 7: LINQ Query on Table
```csharp
// Since SymbolTable implements IEnumerable, use Cast<ObjectId>()
var allIds = table.Cast<ObjectId>().ToList();
```

### Example 8: Checking Ownership
```csharp
// Any record added to this table will have this table as its OwnerID
if (record.OwnerId == table.ObjectId)
{
    // Record belongs to this table
}
```

## Best Practices
1. **Always use Transactions**: Use `AddNewlyCreatedDBObject` immediately after `table.Add()`.
2. **Check `Has()`**: Checks are cheap; exceptions from invalid indexers are expensive.
3. **Erase vs Remove**: Remember there is no `Remove()` method on the table. You `Erase()` the record.
4. **Base Class Usage**: Use `SymbolTable` reference when writing generic code that handles any table (Layers, Linetypes, etc.) identically.

## Related Objects
- [SymbolTableRecord](SymbolTableRecord.md) - The items contained in the table.
- [BlockTable](../SymbolTables/BlockTable.md) - Specific implementation.
- [LayerTable](../SymbolTables/LayerTable.md) - Specific implementation.

## References
- [Autodesk SymbolTable Reference](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_SymbolTable)
