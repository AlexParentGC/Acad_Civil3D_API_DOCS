# RegAppTable Class

## Overview
The `RegAppTable` class is a symbol table that contains all registered application names in an AutoCAD drawing. Applications must register before attaching XData (extended entity data) to objects.

## Namespace
`Autodesk.AutoCAD.DatabaseServices`

## Inheritance Hierarchy
```
System.Object
  └─ RXObject
      └─ DBObject
          └─ SymbolTable
              └─ RegAppTable
```

## Key Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `Has(string)` | `bool` | Checks if an application is registered |
| `this[string]` | `ObjectId` | Gets registered app ObjectId by name (indexer) |

## Code Examples

### Example 1: Registering an Application for XData
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    string appName = "MyApp";
    
    if (!rat.Has(appName))
    {
        rat.UpgradeOpen();
        
        RegAppTableRecord ratr = new RegAppTableRecord();
        ratr.Name = appName;
        
        rat.Add(ratr);
        tr.AddNewlyCreatedDBObject(ratr, true);
        
        ed.WriteMessage($"\nRegistered application '{appName}'");
    }
    
    tr.Commit();
}
```

### Example 2: Listing Registered Applications
```csharp
using (Transaction tr = db.TransactionManager.StartTransaction())
{
    RegAppTable rat = tr.GetObject(db.RegAppTableId, OpenMode.ForRead) as RegAppTable;
    
    ed.WriteMessage("\nRegistered applications:");
    
    foreach (ObjectId appId in rat)
    {
        RegAppTableRecord ratr = tr.GetObject(appId, OpenMode.ForRead) as RegAppTableRecord;
        
        ed.WriteMessage($"\n  {ratr.Name}");
    }
    
    tr.Commit();
}
```

## Related Objects
- [Database](../Core/Database.md) - Contains RegAppTableId
- RegAppTableRecord - Registered application
- XData - Extended entity data

## References
- [Autodesk Official Documentation](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-Autodesk_AutoCAD_DatabaseServices_RegAppTable)
